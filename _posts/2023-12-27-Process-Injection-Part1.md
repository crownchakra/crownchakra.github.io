---
layout: post
title:  "Process Injection Part 1"
---

### Executing payload locally using shellcode.

## Introduction

This method employs Windows APIs like VirtualAlloc, VirtualProtect and CreateThread. No evasion or obfuscation techniques are employed in this; it's simple and straightforward with no emphasis on evasion.

## Section 1: Getting Started

### Utilized functions from the Windows APIs
- VirtualAlloc: Reserves, commits, or changes the state of a region of pages in the virtual address space of the calling process

- RtlMoveMemory: Copies the contents of a source memory block to a destination memory block, and supports overlapping source and destination memory blocks.

- VirtualProtect: Changes the protection on a region of committed pages in the virtual address space of the calling process.

- CreateThread: Creates a thread to execute within the virtual address space of the calling process.

### Flow of the Program (key steps)

- Memory Allocation: A critical step involves reserving and committing memory for the payload using the VirtualAlloc function. The allocated memory, referred to as exec_mem, is initially set to read and write.

```c
void *exec_mem;
BOOL rv;
HANDLE th;
DWORD oldprotect = 0;
exec_mem = VirtualAlloc(0, payload_len, MEM_COMMIT | MEM_RESERVE, PAGE_READWRITE);```

- Copy Payload: The payload is then copied to the newly allocated buffer using the RtlMoveMemory function. This sets the stage for the actual execution.

```c
 RtlMoveMemory(exec_mem, payload, payload_len); ```

- Memory Protection: To enable execution, the protection of the allocated memory is adjusted using VirtualProtect. The memory is now set to be executable and readable, paving the way for the payload's execution.

- Thread Creation: Assuming successful memory protection, a new thread (th) is spawned using CreateThread. The thread is tasked with executing the code residing at the address pointed to by exec_mem. To maintain control flow and observe the execution, the program waits for the created thread to finish using WaitForSingleObject.

```c 
if (rv != 0) {
th = CreateThread(0, 0, (LPTHREAD_START_ROUTINE) exec_mem, 0, 0, 0);
WaitForSingleObject(th, -1);} ```

<iframe width="420" height="315" src="http://www.youtube.com/bW3VQhcYu5A" frameborder="0" allowfullscreen></iframe>


[![Alt text](https://img.youtube.com/vi/bW3VQhcYu5A/0.jpg)](https://youtu.be/bW3VQhcYu5A)

