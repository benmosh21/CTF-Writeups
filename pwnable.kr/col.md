# Writeup: pwnable.kr - col

The "col" challenge on pwnable.kr introduces the concept of hash collisions and basic memory alignment constraints in C. It requires the user to bypass a passcode validation check by understanding how type casting alters the interpretation of data in memory.

![](col1.png)

## Source Code (col.c)

The following is the source code provided for the vulnerable binary:

    #include <stdio.h>
    #include <string.h>
    unsigned long hashcode = 0x21DD09EC;
    unsigned long check_password(const char* p){
        int* ip = (int*)p;
        int i;
        int res=0;
        for(i=0; i<5; i++){
            res += ip[i];
        }
        return res;
    }

    int main(int argc, char* argv[], char* envp[]){
        if(argc<2){
            printf("usage : %s [passcode]\n", argv[0]);
            return 0;
        }
        if(strlen(argv[1]) != 20){
            printf("passcode length should be 20 bytes\n");
            return 0;
        }

        if(hashcode == check_password(argv[1])){
            system("/bin/cat flag");
            return 0;
        }
        else{
            printf("wrong passcode .\n");
            return 0;
        }
    }

## Code Analysis

To successfully solve this challenge and read the flag, we need to satisfy the condition where `check_password(argv[1])` returns a value equal to `hashcode` (`0x21DD09EC`). 

There are two major constraints we must follow:
1. The input string `argv[1]` must be exactly 20 bytes long, enforced by `strlen(argv[1]) != 20`.
2. The input cannot contain null bytes (`\x00`), because `strlen` counts characters until it hits a null byte. A premature null byte would make the length appear shorter than 20 bytes and fail the validation check.

### Understanding the Pointer Cast

Inside `check_password`, the program performs a pointer type cast:
    
`int* ip = (int*)p;`

The input `p` is initially a pointer to a character array (`char*`), where each element is 1 byte. By casting it to an integer pointer (`int*`), the program tells the compiler to treat the underlying memory as an array of 4-byte integers. 

Because the input is 20 bytes long, casting it to an `int*` groups the 20 individual characters into an array of exactly 5 integers ($20 \div 4 = 5$). The subsequent `for` loop sums these 5 integers together into the `res` variable.

## Calculating the Passcode

Our objective is to find 5 four-byte integers that sum up to `0x21DD09EC` without any of the individual bytes evaluating to `0x00`.

Let's convert `0x21DD09EC` to its decimal equivalent:
`0x21DD09EC` = 568134124

Dividing this target value by 5 gives us a baseline integer value:
568134124 / 5 = 113626824 with a remainder of 4.

This means we can use four identical integers equal to 113626824, and one final integer that absorbs the remainder:
* Integers 1 to 4: 113626824
* Integer 5: 113626824 + 4 = 113626828

Let's convert these back to hexadecimal format:
* 113626824 = `0x06C5CEC8`
* 113626828 = `0x06C5CECC`


### Handling Endianness

Modern x86 architectures utilize Little-Endian representation, meaning the least significant byte is stored at the lowest memory address. When passing the bytes via the command line, we must reverse the order of bytes for each integer:
* `0x06C5CEC8` becomes `\xc8\xce\xc5\x06`
* `0x06C5CECC` becomes `\xcc\xce\xc5\x06`

None of these bytes are `0x00`, so they will safely pass the `strlen` check without truncating the input.

## Execution & Flag Extraction

We can use an inline Python script to construct the 20-byte payload and pass it directly as an argument to the binary:

    ./col $(python -c "print '\xc8\xce\xc5\x06'*4 + '\xcc\xce\xc5\x06'")

Executing this command feeds the constructed integers into the program, triggering the hash collision, and successfully printing out the flag.

![alt text](col2.png)