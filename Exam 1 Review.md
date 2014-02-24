Introduction
===

__Multiprogramming__ : Keep several programs in memory at once and switch between them.

__System Call__ : Uses the `trap` mechanism to switch control to operating system code running in kernel mode.

__Batch Systems__ : Operating systems that would run a program to completion and then load and run the next program.

__Device Drivers__ : A `computer program` that operates or controls a particular type of device that is attached to a computer.

__Preemption__ : The system may stop one process from executing and starts another (used in `Time-sharing`).

__Operating System__ : What does it do?
* It is a program that loads and runs other programs.
* It provides programs with a level of abstraction so they don't have to deal with the details of accessing hardware.
* It manages access to resource, including the `CPU` (via the scheduler), `memory` (via the memory management unit), `persistent files` (via the file system), `communications network` (via sockets and IP drivers), and `devices` (via device drivers).

__Mechanism__ versus __Policy__
* A `Mechanism` is the presentation of a software abstraction: the functional interface. It defines _how to do something_.
* A `Policy` defines how that mechanism behaves (e.g., enforce permissions, time limits, or goals).
* Good design dictates that we keep _mechanisms and policies separate_.

Booting
===

__Boot Loader__ : A small program that is run at boot time that loads the operating system.

__Multi-stage Boot Loader__ : Starts off with a simple boot loader that then loads a more sophisticated boot loader that then loads the operating system. This cascaded boot sequence is called `chain loading`.

__What the BIOS does__
* Performs a _power-on test_
* _identifies and initializes_ some hardware
* Loads the _first block_ (MBR) of the disk

__Master Boot Record__ (MBR)
* Contains a boot loader that loads the volume boot loader from a disk partition.
* MBR contains the stage 1 loader that then loads the stage 2 loader, which then loads the `Operating System` (or gives you a choice).

__Volume Boot Record__ (VBR)
* The _first disk block_ of the designated boot partition
* Present user with a choice of operating systems to load

__Universal Extensible Firmware Interface__ (EFI)
* A successor to the BIOS
* Has a built-in boot manager that usually loads an OS loader without going through a two-stage process.

OS Concepts
===

__Monolithic__ vs __Modular__ vs __Microkernel__ structures
* A `Monolithic Kernel` is an operating system architecture where the entire operating system is working in kernel space and is alone in supervisor mode.
* A `Loadable Kernel Module` is an object file that contains code to extend the running kernel of an operating system.
* A `Microkernel` is an operating system that provides just the bare essential mechanisms that are necessary to interact with the hardware and manage threads and memory.

__User Mode__ (Privileged) vs __Kernel Mode__ (Supervisor)
* Programs other than the kernel run with the processor in `user mode` and do not have privileges to execute these instructions.
* The operating system runs with the processor in `kernel mode`.

__Getting from user to kernel mode and back again__ : A processor running in `user mode` can switch to kernel mode by executing a `trap` instruction (`software interrupt`).

__Traps__ (Software Interrupts)
* `INT` a legacy way to invoke a system call and should be avoided
* `SYSCALL` explicit instructions, which is a faster mechanism since it does not need to read the branch address from a `interrupt vector table` that is stored in memory but keeps the address in a CPU register.

__System Calls__ : a system call uses the `trap` mechanism to switch control to operating system code running in kernel mode.

__Programmable Interval Timer Interrupts__ : Why do you need them? 
* To allow the operating system to get control at regular intervals.
* Generate periodic hardware interrupts (i.e. every 10ms).

__Mode Switch__ vs __Context Switch__
* An interrupt or trap results in a `mode switch`, where execution is transferred from _user mode to kernel mode_.
* Saving the context of one process and restoring that of another is called a `context switch`. The operating system decides if it is time to replace the currently executing processes with another process.

__What's involved in saving process state?__  
It comprises the state of the processor's registers and the `memory map` for the program. 

__Device type__ : Character, Block, and Network
* `Character Devices` any device whose data that can be thought of as a byte stream. This includes keyboard input, mouse movements, printer output, camera inputs, etc.
* `Block Devices` any device that has persistent storage that is randomly addressable and read or written in fixed-size chunks (blocks). These devices include disks and flash memory. Because the data is persistent and addressable, it can be cached by the operating system so that future requests for cached content may be satisfied from system memory instead of accesssing the device again. This cache of frequently-used blocks of data is called the `buffer cache`. Basically, any device that can be used to hold a file system is a block device. 
* `Network Devices` packet-based communications networks.

__Interacting with Devices__ : Memory Mapped I/O
* Exists within a `process`
* Creates and initializes the following when first created:
	* `text` the machine instructions
	* `data` initialized static and global data
	* `bss` uninitialized static data that was defined in the program
	* `heap` dynamically allocated memory (obtained through memory allocation requests)
	* `stack` the call stack, which holds not just return addresses but also local variables, temporary data, and saved registers

__Getting status from devices__ : Interrupts and Polling 
The processor can get device status notifications either via a `hareware interrupt` or by `periodically polling` the hardware.

__Moving data to/from devices__ : Programmed I/O or DMA
* `Programmed I/O` data can be transferred between the device and system via software that reads/writes the device's memory.
* `Direct Memory Access` transfer data to/from system memory without using the processor.




