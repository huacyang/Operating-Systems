Introduction
===

* __Batch Systems__: operating systems that would run a program to completion and then load and run the next program.

* __Multiprogramming__: keep several programs in memory at once and switch between them.

* __Time-sharing__: multiprogramming with preemption, where the system may stop one process from executing and starts another.

* __Portable Operating System__: an operating system that is not teid to a specific hardware platform. UNIX was one of the early portable operating systems. Windows NT was also written to be architecture-independent.

* __Microkernel Operating System__: an operating system that provides just the bare essential mechanisms that are necessary to interact with the hardware and manage threads and memory. Higher-level operating system funcitons, such as managing file systems, the network stack, or scheduling policies, are delegated to user-level processes that communicate with the microkernel via interprocess communication mechanisms (usually messages).

A __mechanism__ is the presentation of a software abstraction: the functional interface. It defines how to do something. A __policy__ defines how that mechanism behaves (e.g., enforce permissions, time limits, or goals). Good design dictates that we keep mechanisms and policies separate.

