---
title:  Linux/86 Helloword -> Stack,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to print Hello World`

Platform: `Linux`

Arch: `x86`

Size: `36 bytes`

Technique: `Stack`, `XOR`

Shellcode : `\x68\x72\x6c\x64\x0a\x68\x6f\x20\x57\x6f\x68\x48\x65\x6c\x6c\x31\xc0\x31\xdb\x31\xd2\xb0\x04\xb3\x01\x8d\x0c\x24\xb2\x0c\xcd\x80\xb0\x01\xcd\x80`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/l86hellosstackxor.png)

# Assembly-nasm

`nasm -f elf32 -o shellcode.o shellcode.asm`

`ld -o shellcode shellcode.o`

```nasm

global _start

section .text

_start:

    ; Hello World
    
    push 0x0a646c72
    push 0x6f57206f
    push 0x6c6c6548

    xor eax, eax
    xor ebx, ebx
    xor edx, edx

    mov al, 0x4
    mov bl, 0x1
    lea ecx, [esp]
    mov dl, 0xc
    int 0x80

    mov al, 0x1
    int 0x80

```
# objdump

`objdump -d ./shellcode -M intel`

```bash

./shellcode:     file format elf32-i386


Disassembly of section .text:

08048060 <_start>:
 8048060:	68 72 6c 64 0a       	push   0xa646c72
 8048065:	68 6f 20 57 6f       	push   0x6f57206f
 804806a:	68 48 65 6c 6c       	push   0x6c6c6548
 804806f:	31 c0                	xor    eax,eax
 8048071:	31 db                	xor    ebx,ebx
 8048073:	31 d2                	xor    edx,edx
 8048075:	b0 04                	mov    al,0x4
 8048077:	b3 01                	mov    bl,0x1
 8048079:	8d 0c 24             	lea    ecx,[esp]
 804807c:	b2 0c                	mov    dl,0xc
 804807e:	cd 80                	int    0x80
 8048080:	b0 01                	mov    al,0x1
 8048082:	cd 80                	int    0x80

```

# C

`gcc -o shellcode shellcode.c`

```c

#include <stdio.h>
#include <string.h>

char *shellcode = "\x68\x72\x6c\x64\x0a\x68\x6f\x20\x57\x6f\x68\x48\x65\x6c\x6c\x31\xc0\x31\xdb\x31\xd2\xb0\x04\xb3\x01\x8d\x0c\x24\xb2\x0c\xcd\x80\xb0\x01\xcd\x80";
int main(void)
{
	fprintf(stdout,"Length: %d\n",strlen(shellcode));
	(*(void(*)()) shellcode)();
	return 0;
}

```

