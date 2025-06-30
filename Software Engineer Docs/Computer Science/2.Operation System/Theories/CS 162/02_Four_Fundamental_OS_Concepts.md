# CS162 Lecture 2: Four Fundamental OS Concepts

---

This lecture delves into four foundational concepts that underpin the design and operation of modern operating systems: Threads, Address Spaces, Processes, and Dual Mode Operation/Protection. These concepts are crucial for understanding how an OS manages resources, ensures security, and enables concurrent execution.

## I. Threads: Execution Context

A **thread** is defined as a single, unique execution context within a program.1 It represents an independent sequence of instructions that can be scheduled and executed by the processor. The essential components that define a thread's state include:

- **Program Counter (PC):** Points to the next instruction to be executed.1
- **Registers:** Hold the root state of the thread, including general-purpose registers and special-purpose registers.1
- **Execution Flags:** Indicate the status of the processor after an operation.1
- **Stack:** A region of memory used to store local variables, function call parameters, and return addresses.1

A thread is considered "executing" when its data (PC, registers, stack pointer) is loaded into the processor's registers and the processor's PC points to its next instruction.1 When a thread is not actively running, its state is saved in a **Thread Control Block (TCB)** in memory.

The operating system creates the illusion of multiple processors by rapidly switching between threads, a process known as

**context switching**, which involves saving one thread's state and loading another's.1

Threads are fundamental for enabling **multiprogramming** and **concurrency**.1 They allow the OS to handle multiple tasks simultaneously, such as managing processes, responding to interrupts, performing background system maintenance, and handling concurrent requests in networked servers.1 For programs with user interfaces, threading is essential for responsiveness, preventing the application from freezing during long operations.1

It's important to distinguish between **concurrency** and **parallelism**.1 Concurrency refers to handling multiple things at once, while parallelism means doing multiple things at the same time, typically on multiple CPU cores.1 Parallel execution implies concurrency, but concurrency does not necessarily imply parallelism (e.g., multiple threads on a single-core CPU are concurrent but not parallel).1

## II. Address Spaces (with Translation)

An **address space** is a crucial OS concept that defines the set of memory addresses a thread or process can access.1 This provides a program with its own distinct view of memory, separate from the physical memory layout of the machine.2 For a 32-bit processor, an address space can encompass approximately 4 billion addresses, while a 64-bit processor can access significantly more.1

When a program attempts to read or write to an address, it can result in various behaviors:

- Accessing a regular memory location for data storage.1
- Triggering an I/O operation through **memory-mapped I/O**.1
- Causing the program to terminate due to an invalid access (e.g., a **segmentation fault** or "segfault").1
- Facilitating communication between different programs.1

A typical address space is structured into several segments:

- **Code Segment:** Contains the executable instructions of the program.1
- **Static Data:** Stores data that remains constant throughout the program's execution, such as global variables.1
- **Heap:** Used for dynamically allocated memory, which typically grows upwards.1
- **Stack Segment:** Used for local variables and function call management, typically growing downwards.1

To ensure protection and efficient memory management, operating systems employ **address translation**.1 An early, simpler model for protection is

**"Base and Bound"**, where each program is allocated a contiguous block of memory defined by a base (start) address and a bound (end) address.1 The OS checks if every memory access falls within these bounds; if not, the process can be terminated.1 However, this model suffers from fragmentation issues.1

A more advanced and widely used method is **Paged Virtual Address Space**.1 In this approach, the virtual address space is divided into equal-sized chunks called

**pages**.1 Each virtual page can be mapped to any physical memory frame of the same size.1 Hardware components, using a

**Page Table**, translate the virtual addresses used by the program into physical addresses.1 A special hardware register stores a pointer to the page table for the currently running process.1 This allows the OS to treat memory as a collection of pages that can be flexibly placed into physical memory frames, greatly reducing fragmentation and enabling efficient memory sharing and protection.1

## III. Processes: An Instance of a Running Program

A **process** is defined as an isolated execution environment with restricted rights.1 It encapsulates a running program and all the resources it needs to execute.1 Key components of a process include:

- **Address Space:** A distinct memory region isolated from other processes, providing memory protection.1
- **One or more Threads:** Processes contain one or more threads, which are the actual units of execution.1 Threads within the same process share its address space and other resources, enabling concurrency.1
- **Open Files:** A list of files the process currently has open.1
- **Open Sockets:** Network connections established by the process.1
- **File System Context:** Information about the process's current working directory and other file system-related states.2

Processes are fundamental to the operating system's ability to manage multiple applications concurrently and securely.1 They are designed to be protected from each other, meaning a bug or crash in one process typically cannot directly affect or crash other processes or the OS itself.1 This isolation is critical for system stability and reliability.

While threads primarily encapsulate **concurrency** (the active component of execution), address spaces encapsulate **protection** (the passive component).1 This design allows for efficient multi-tasking where multiple threads can run within a protected environment, preventing buggy programs from corrupting the entire system.1 Even though kernel code and data might be mapped into a process's virtual address space at high addresses, they are inaccessible to user processes, further reinforcing protection.2

## IV. Dual Mode Operation / Protection

**Dual mode operation** is a fundamental hardware-supported mechanism that allows the operating system to protect itself from user programs and to protect user programs from each other.1 This protection is vital for system reliability (buggy programs only harm themselves), security and privacy (limiting trust in programs), and fairness (enforcing resource shares).2

The hardware provides at least two distinct modes of operation:

- **Kernel Mode ("Supervisor Mode"):** This is a highly privileged mode where the operating system kernel executes.1 In kernel mode, the OS has unrestricted access to all hardware resources and can execute any instruction, including privileged ones.1
- **User Mode:** This is a less privileged mode where user applications and most system utilities run.1 In user mode, certain sensitive operations are prohibited, such as directly manipulating hardware, changing page table pointers, disabling interrupts, or accessing memory outside its allocated address space.1

Transitions between user mode and kernel mode are strictly controlled and can only occur through specific, predefined mechanisms 1:

1. **System Calls (Syscalls):** A process explicitly requests a service from the operating system, such as performing file I/O, allocating memory, or creating/terminating other processes.1 These requests act as controlled entry points into the kernel, where arguments are passed and results are returned.2
2. **Interrupts:** These are asynchronous, external events generated by hardware (e.g., a timer expiring, an I/O device completing an operation) that cause an immediate transfer of control to the OS.2 Interrupts are independent of the user process's execution.2
3. **Traps (Exceptions):** These are synchronous, internal events generated by the process itself due to an error or an exceptional condition (e.g., a division-by-zero error, an invalid memory access like a segmentation fault, or an attempt to execute a privileged instruction in user mode).1 Traps also transfer control to the OS for handling.2

All three types of transfers are "unprogrammed control transfers" that direct execution to a predefined **Interrupt Vector Address**.2 This prevents user programs from specifying the destination of the transfer, thereby protecting the kernel.2 When a transfer occurs, the hardware automatically switches to kernel mode and saves the user's Program Counter (PC).2 The OS can then save the rest of the user's state if needed.2

The kernel uses a separate **kernel stack** (located in kernel memory) for handling interrupts and system calls, as the user stack cannot be trusted or used for privileged operations.2 Each OS thread typically has its own kernel stack.2 Kernel system call handlers carefully validate and copy arguments from user memory to kernel memory before processing them, and then copy results back to user memory.2 Interrupt processing is designed to be transparent to the user process, occurring between instructions and allowing the process to resume execution seamlessly once the interrupt is handled.2

These four fundamental concepts—Threads, Address Spaces, Processes, and Dual Mode Operation—work in concert to form the bedrock of modern operating systems, enabling them to manage complex computing environments efficiently, securely, and reliably.