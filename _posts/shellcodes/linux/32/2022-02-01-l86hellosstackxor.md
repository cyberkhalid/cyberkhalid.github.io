---
title:  Linux/86 Helloword -> Jmp-call-pop,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to print Hello World`

Platform: `Linux`

Arch: `x86`

Size: `38 bytes`

Technique: `Jmp-call-pop`, `XOR`

Shellcode : `\xeb\x13\x59\x31\xc0\x31\xdb\x31\xd2\xb0\x04\xb3\x01\xb2\x0c\xcd\x80\xb0\x01\xcd\x80\xe8\xe8\xff\xff\xff\x48\x65\x6c\x6c\x6f\x20\x57\x6f\x72\x6c\x64\x0a`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/l86hellosstackxor.png)

# Assembly-nasm

`nasm -f elf32 -o shellcode.o shellcode.asm`

`ld -o shellcode shellcode.o`

```nasm

global _start

section .text


_start:
    jmp call_shell

shell:
    pop ecx

    xor eax, eax
    xor ebx, ebx
    xor edx, edx

    mov al, 0x4
    mov bl, 0x1
    mov dl, 0xc
    int 0x80

    mov al, 0x1
    int 0x80

call_shell:
    call shell
    msg: db "Hello World", 0xA


```
# objdump

`objdump -d ./shellcode -M intel`

```bash

./shellcode:     file format elf32-i386


Disassembly of section .text:

08048060 <_start>:
 8048060:       eb 13                   jmp    8048075 <call_shell>

08048062 <shell>:
 8048062:       59                      pop    ecx
 8048063:       31 c0                   xor    eax,eax
 8048065:       31 db                   xor    ebx,ebx
 8048067:       31 d2                   xor    edx,edx
 8048069:       b0 04                   mov    al,0x4
 804806b:       b3 01                   mov    bl,0x1
 804806d:       b2 0c                   mov    dl,0xc
 804806f:       cd 80                   int    0x80
 8048071:       b0 01                   mov    al,0x1
 8048073:       cd 80                   int    0x80

08048075 <call_shell>:
 8048075:       e8 e8 ff ff ff          call   8048062 <shell>

0804807a <msg>:
 804807a:       48                      dec    eax
 804807b:       65 6c                   gs ins BYTE PTR es:[edi],dx
 804807d:       6c                      ins    BYTE PTR es:[edi],dx
 804807e:       6f                      outs   dx,DWORD PTR ds:[esi]
 804807f:       20 57 6f                and    BYTE PTR [edi+0x6f],dl
 8048082:       72 6c                   jb     80480f0 <msg+0x76>
 8048084:       64                      fs
 8048085:       0a                      .byte 0xa

```

# C

`gcc -o shellcode shellcode.c`

```c

#include <stdio.h>
#include <string.h>

char *shellcode = "\xeb\x13\x59\x31\xc0\x31\xdb\x31\xd2\xb0\x04\xb3\x01\xb2\x0c\xcd\x80\xb0\x01\xcd\x80\xe8\xe8\xff\xff\xff\x48\x65\x6c\x6c\x6f\x20\x57\x6f\x72\x6c\x64\x0a";

int main(void)
{
        fprintf(stdout,"Length: %d\n",strlen(shellcode));
        (*(void(*)()) shellcode)();
        return 0;
}


```

