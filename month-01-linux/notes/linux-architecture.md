# 1.Linux architecture

The linux architecture is a system with several layers, with different responsibilities.

The USER mode is combined of: 
    The user interface: apps like: (browser, Vs Code, Docker),
    The library interface: Standard utility programs like: (shell, editors, compiler, etc.),
    The system call interface: Standard library like: (open, close, read, write, fork, etc.).
The kernel mode:
    Linux operating system: (process management, memory management, file system, I/O).
The hardware:
    (CPU, memory, disks, terminals).

# 2.Kernel

The KERNEL is the core of the OS (manages the communication between the software and hardware)

The KERNEL manages important things like:
    process management: "Creates, schedules and terminates processes",
    memory management: "Allocates and manages RAM",
    file systems: "Handles reading and writing files",
    device drivers: "communicates with hardware",
    networking: "manages network communication",
    security: "controls permissions and access".

## 3.System call 

A system call is a mechanism that allows a program to request services from the kernel.
open() — open a file. | read() — read data.
write() — write data. | fork() — create a process.
execve() — execute a program.

# 4.Shell 

The shell receives commands from the user, interprets them and executes programs.

# 5.Terminal

The terminal is the interface through which you interact with a shell.

# 6. Linux distributions

A distribution combines the Linux kernel with other software and tools to provide a usable operating system.



