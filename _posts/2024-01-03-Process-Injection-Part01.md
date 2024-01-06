---
layout: post
title: "Exploring Process Injection - Part 1"
---

## Injecting Shellcode into a Remote Process

### Introduction

Process injection is a powerful technique used in both legitimate applications and malicious software, involving the manipulation of a running process's memory space. In this blog post, we will delve into the intricacies of remote process injection using a simple C program for educational purposes. It's important to note that the provided code lacks evasion techniques and is intended solely for educational exploration.

### Overview of Process Injection

Process injection serves as a potent method for executing code in the address space of another process. This allows for various functionalities, including code injection, DLL injection, and reflective DLL injection. In our exploration, we'll focus on remote process injection, where the injection process is initiated from an external program.

### Utilized Functions from the Windows APIs

The success of remote process injection hinges on leveraging specific Win32API functions. These functions play pivotal roles in tasks such as process identification, memory allocation in the remote process, and thread creation. Here are key Win32API functions employed in the provided C code:

- `CreateToolhelp32Snapshot`: Captures a snapshot of all processes currently running in the system.
- `Process32First` and `Process32Next`: Retrieve information about the first and subsequent processes in the snapshot.
- `OpenProcess`: Opens an existing process, providing various access rights.
- `VirtualAllocEx`: Allocates memory in the address space of the specified process.
- `WriteProcessMemory`: Writes data to the memory of the specified process.
- `CreateRemoteThread`: Creates a thread running in the address space of the specified process.

### Flow of the Program (Key Steps)

1. **Process Identification:** Identify the process ID of "Notepad.exe" or any process ID of your choice using `FindProcessId`.
2. **Open the Process:** If the process ID is obtained, open the process with full access rights using `OpenProcess`.
3. **Payload Injection:** If the process is successfully opened, inject the payload into the target process via the `injectPayload` function.
4. **Execution:** The injected payload, represented by the shellcode, is executed within the context of the target process.

### Understanding the Code

#### Payload Definition

The payload variable encapsulates a block of machine code, acting as the payload intended for injection. This payload, in our example, is designed to open a calculator.

```c
unsigned char payload[] = {
  // ... (payload content)
};
unsigned int payload_len = sizeof(payload);
```