pwneasy by benmosh
==================


by [BenMosh](https://medium.com/@guybmontop?source=post_page---byline--833f9ab374a4---------------------------------------)




The `pwneasy` challenge is a Linux binary exploitation task. It requires bypassing a size restriction using an integer type confusion vulnerability to trigger a stack-based buffer overflow and hijack the execution flow to a win function.

Initial Analysis
----------------

Running the binary normally shows a basic input/output flow. It leaks the address of a function called `print_flag`, asks for a size, reads the data, and cleanly exits.

![captionless image](https://miro.medium.com/v2/format:webp/1*NG-UWOGfxsEoc-XyjrzIyw.png)


Vulnerability Analysis
----------------------

The core issue lies in how the program handles the input size between the `main` function and the `vuln` function. In `main`, the program takes the user's requested size and stores it in a signed integer. It then performs a security check to ensure the size does not exceed the 64-byte buffer limit.

```
int size;
if (scanf("%d", &size) != 1) {
    puts("Invalid number.");
    exit(1);
}
if (size > BUF_SIZE) {
    puts("Too big!");
    return 1;
}
```

If a negative number like `-1` is provided, it easily passes this check because `-1` is less than 64. The execution then moves to the `vuln` function, which takes the size as an argument. However, the signature for `vuln` expects an `unsigned short`.

```
void vuln(unsigned short count)
```

When the signed `-1` is passed as an `unsigned short`, the binary interprets it using two's complement. A 16-bit signed `-1` translates to the maximum unsigned value, which is 65535. This completely breaks the boundaries of the `read` function inside `vuln`.

```
char buf[BUF_SIZE];
ssize_t n = read(STDIN_FILENO, buf, count);
```

Because `count` is now 65535, the `read` function will accept up to 65535 bytes into a 64-byte buffer. This allows for a massive stack overflow, giving full control over the saved instruction pointer (RIP).

Exploit Development
-------------------

To successfully hijack the execution flow, the payload needs to reach the saved RIP. A simple look at the 64-byte buffer might suggest a 72-byte offset (64 bytes for the buffer plus 8 bytes for the saved base pointer). However, modern compilers allocate stack space dynamically.

Inside `vuln`, there is an 8-byte local variable `ssize_t n`. Additionally, GCC enforces 16-byte stack alignment, which adds dummy padding bytes to the stack frame. Because of this extra space, the actual distance from the start of the buffer to the saved RIP is exactly 88 bytes.

The payload structure must include the bypass number, a newline to flush the `scanf` buffer, the 88 bytes of padding, and finally the target address in little-endian format. The target address is the leaked `print_flag` location at `0x4011f6`.

```
import sys
sys.stdout.buffer.write(b'-1\n' + b'A'*88 + b'\xf6\x11\x40')
```

The `b'-1\n'` handles the size prompt. The `88` 'A's fill the buffer, the local variable space, the alignment padding, and overwrite the saved RBP. The trailing bytes overwrite the RIP with the target function's address. The total payload length read by the vulnerable function will be 91 bytes (88 bytes of padding + 3 bytes for the address).

Execution
---------

Piping the Python payload into the running Docker container triggers the overflow. The binary reads the 91 bytes, crashes the normal return path, and jumps straight to the flag function.

![captionless image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*D0luRaFlCekwQ0Bn-yTJ9Q.png)

The exploit lands perfectly, bypassing the security check and returning the flag: `ICO{sign_mismatch_is_so_confusing}`.
