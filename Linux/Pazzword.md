Pazzword by Benmosh
===================


by [BenMosh](https://medium.com/@guybmontop?source=post_page---byline--e22aafb70c22---------------------------------------)




part 1: recon and almost getting
--------------------------------

first thing I did was run some basic commands in kali to see what I was dealing with. `strings pazzword | grep -i "ctf"` the strings didn't reveal anything useful, so I ran the file command: `file pazzword`

it gave me this output: `pazzword: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, with debug_info, not stripped`

because it said `statically linked` and `not stripped`, I knew all the original function names were still in there. so I booted up gdb to check the main function. `(gdb) disassemble main` `No symbol "main" in current context.`

at first I thought I was cooked. but then I realized this wasn’t a normal C program, it was written in zig! zig handles entry points differently. so I told gdb to just find anything with “main” in the name: `(gdb) info functions main`

and boom, it spit out the exact checklist the dev used to check the password: `main.assert_length` `main.assert_ICO` `main.assert_xor_middle` `main.assert_checksum_suffix`

part 2: finding the length
--------------------------

I jumped into the first check to see how long the password needed to be: `(gdb) disassemble 'main.assert_length'`

I tracked the length variable and saw it went through three math checks:

1.  `and $0x3,%rax` - it did a bitwise AND with 3, which is a modulo 4 check. so the length had to be a multiple of 4.
2.  `div %rcx` (divide by 3) and `cmp $0x0,%rdx` - it checks if the remainder is 0 and jumps to a fail block if it is. so it CANNOT be a multiple of 3.
3.  `div %rcx` (divide by 6) and `cmp $0x2,%rax` - it checks if the quotient is 2. so the length has to be between 12 and 17.

a number between 12 and 17, that is a multiple of 4, but not a multiple of 3? the length had to be exactly 16.

part 3: the prefix
------------------

next I checked the second constraint: `(gdb) disassemble 'main.assert_ICO'`

since it’s zig, it didn’t just compare a giant hex string. it set up a loop to check the input byte by byte. I saw it load the length (4) and a memory address `0x1025bdc` which pointed to the secret string.

instead of reading all the loop assembly, I just used the examine command to read the memory directly: `(gdb) x/s 0x1025bdc` `0x1025bdc <__anon_26853>: "ICO{"`

so the first 4 chars are `ICO{`.

part 4: reversing the xor cipher
--------------------------------

then came the middle part of the password: `(gdb) disassemble 'main.assert_xor_middle'`

I saw another loop with an `xor` instruction. it was scrambling my input with a key located at `0x1025b62`. after the loop, it called a zig function `mem.eql` to compare my scrambled input against the real ciphertext stored at `0x1021bfa` (which was 8 bytes long).

because xor is reversible, I didn’t need to guess anything. I just dumped the 8 bytes for both the key and the cipher from gdb: `(gdb) x/8xb 0x1025b62` (this was the key: 0x44 0x45 0x41 0x44 0x62 0x65 0x65 0x66) `(gdb) x/8xb 0x1021bfa` (this was the cipher: 0x74 0x36 0x24 0x27 0x10 0x00 0x11 0x5f)

then I wrote a quick python script to reverse the math:

Python

```
key = [0x44, 0x45, 0x41, 0x44, 0x62, 0x65, 0x65, 0x66]
cipher = [0x74, 0x36, 0x24, 0x27, 0x10, 0x00, 0x11, 0x5f]
``````
plaintext = ""
for i in range(8):
    plaintext += chr(key[i] ^ cipher[i])print(plaintext)
```

it printed `0secret9`. so now my flag was `ICO{0secret9` with 4 characters left.

part 5: the checksum monster
----------------------------

the last function was a massive wall of math: `(gdb) disassemble 'main.assert_checksum_suffix'`

I looked for context clues to make it easier. at the start of the assembly, it took character 12 and character 15, xored them, and compared the result to `0x19`:

Code snippet

```
0x00000000010e66b1 <+225>:   xor    %cx,%ax
0x00000000010e66b4 <+228>:   cmp    $0x19,%ax
```

since I know the ctf flag format, character 15 has to be the closing brace `}` (which is 0x7d in hex). so `char_12 ^ 0x7d = 0x19`. I did the math backwards (`0x19 ^ 0x7d`) and got 0x64, which is the letter `d`! now the end looked like `d??}`.

the rest of the assembly was a crazy equation using div and mul to check the two unknown characters. I didn’t want to solve it by hand, so I mapped the registers to a math equation and wrote a python script to brute force the last two characters using the known ones (c0=100 for ‘d’, c3=125 for ‘}’):

Python

```
for c1 in range(32, 127):
    for c2 in range(32, 127):
        if c2 == 0 or c1 == 0: continue
        
        q1 = (100 * c1) // (c2 * 2)
        q2 = (100 * c2) // (c1 * 2)
        q3 = (c1 * c2) // 200
        
        if (q3 + q1) - q2 == 125:
            print(chr(c1), chr(c2))
```

ran the script and it spit out `y` and `5`.

final flag
----------

put it all together and the crackme is solved: `ICO{0secret9dy5}`
