---
title:  Linux/86 Create Directory (mkdir)-> Jmp-call-pop,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to create a directory.`

Platform: `Linux`

Arch: `x86`

Size: `29 bytes`

Technique: `jmp-call-pop`, `XOR`

Shellcode : `\xeb\x13\x5b\x31\xc0\x31\xc9\xb0\x27\x66\xb9\xc0\x01\xcd\x80\x31\xdb\xb0\x01\xcd\x80\xe8\xe8\xff\xff\xff\x70\x77\x6e`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/mkdirjmpcallpopxor.png)

# Assembly-nasm

`nasm -f elf32 -o shellcode.o shellcode.asm`

`ld -o shellcode shellcode.o`

```nasm

global _start

section .text

_start:
    
    jmp data

main_:
    pop ebx
   
   ; create directory
    xor eax, eax
    xor ecx, ecx
    mov al, 0x27
    mov cx, 0q700
    int 0x80

    ; exit
    xor ebx, ebx
    mov al, 0x1
    int 0x80

data:
    call main_
    dirname: db "pwn"
 

```
# objdump

`objdump -d ./shellcode -M intel`

```bash

./shellcode:     file format elf32-i386


Disassembly of section .text:

08048060 <_start>:
 8048060:       eb 13                   jmp    8048075 <data>

08048062 <main_>:
 8048062:       5b                      pop    ebx
 8048063:       31 c0                   xor    eax,eax
 8048065:       31 c9                   xor    ecx,ecx
 8048067:       b0 27                   mov    al,0x27
 8048069:       66 b9 c0 01             mov    cx,0x1c0
 804806d:       cd 80                   int    0x80
 804806f:       31 db                   xor    ebx,ebx
 8048071:       b0 01                   mov    al,0x1
 8048073:       cd 80                   int    0x80

08048075 <data>:
 8048075:       e8 e8 ff ff ff          call   8048062 <main_>

0804807a <dirname>:
 804807a:       70 77                   jo     80480f3 <dirname+0x79>
 804807c:       6e                      outs   dx,BYTE PTR ds:[esi]


```

# C

`gcc -o shellcode shellcode.c`

```c

#include <string.h>
#include <stdio.h>

const char * shellcode = "\xeb\x13\x5b\x31\xc0\x31\xc9\xb0\x27\x66\xb9\xc0\x01\xcd\x80\x31\xdb\xb0\x01\xcd\x80\xe8\xe8\xff\xff\xff\x70\x77\x6e";

int main(void){
        printf("Length: %d\n", strlen(shellcode));
        (*(void(*)())shellcode)();
        return 0;
}


```

