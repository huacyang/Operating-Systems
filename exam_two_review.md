## Memory Management

#### Static vs. dynamic linking, shared libraries

* Static linking is the result of copying all library routines used in the program into the executable image
  - Disadvantage: May require more disk space and memory
  - Advantage: Faster and more portable, since it does not require the presence of the library ont he system where it is run
* Dynamic linking is accomplished by placing the name of a sharable library in the executable image
  - Actual linking with the library does not occur until run, where both the executable and library are placed in memory
  - Advantage: multiple programs can share a single copy of the library

#### Monoprogramming vs multiprogramming

* Monoprogramming can only load one process at a time
* Multiprogramming can load multiple processes at a time

#### Single partition monoprogramming

* Only a single program can be loaded in memory at any given time
* The program shares its memory with the OS and nothing else

#### CPU utilization

* The estimated percentage of time taht the CPU is not idle

#### Degree of multiprogramming

* The number of processes in memory at one time (and candidates for running)

#### Dynamically relocatable code

* _Position-independent code_ is a body of machine code
  - May be loaded into any memory location
  - Executes properly regardless of its absolute address
  - Commonly used for shared libraries

#### Memory management unit: concept of dynamic address translation

* A computer hardware unit having all memory references passed through itself
* Primarily performing the translation of virtual memory addresses to physical addresses

#### Logical (virtual) vs Physical (real) addresses

* _Absolute code_ use real memory addresses  
  - Expects to be loaded into and run from a specific memory location
  - Risk: some absolute memory references may have been used
  - Alternative: generate a _relocation table_ that allow references to be filled when program is loaded
* _Logical addressing_ use virtual memory addresses
  - Rely on hardware to dynamically transform address references made by process into actual memory locations
  - This translation is performed by the _memory management unit_

#### Base & limit addressing

* Requires each process to occupy contiguous memory

#### Multiple fixed partitions (internal vs. external fragmentation)

* The OS divides memory into a bunch of partitions ahead of time
* When a program needs to be loaded, it is loaded into an available partition that can hold it
  - If no partition is available, the loading will have to be delayed in a queue of requests
* _Internal fragmentation_
  - Unused space within a partition remains unused
* _External fragmentation_
  - If any partitions have no programs to run, then their memory is always wasted


#### Kernel memory allocation

* __Page allocator (why might you need contiguous pages?)__
* __Buddy system__
* __Slab allocator (know how it works)__

#### Page-based virtual memory

* __Page frames__
  - Physical memory is divided into equal-sized chunks that are multiple of a power of two (byte offset)
* __Page number and offset (displacement)__
  - The high-order bits of the address identify a page number
  - The low-order bits of the address identify a page offset
* __Page table, PTE, page fault__
  - For each memory reference made by the processor, the MMU takes the logical address, extracts the page number, and uses that as an index into a page table
  - Each _page table entry_ (PTE) contains a page frame number
* __Direct mapping paging system, page table base register__
* __Translation lookaside buffer (TLB)__
* __Hit ratio__
* __Address space identifier: why do you want it?__
* __Multilevel (hierarchical) page tables, partial page tables (understand benefits)__
* __Inverted page table__

#### Understand the benefits of page-based virtual memory

#### ARM MMU

* __Pages (via a two-level hierachy) and sections (via direct mapping)____
* __Two levels of TLB__
* __Understand the value of sections in having an efficient way to map large chunks of memory (operating system -> process' address space)__

#### Intel MMU

* __It supports combined paging & segmentation__
* __What's a far pointer?__
  - A combined segment value and offset
* __Segmentation, offering per-segment protection__
* __Depending on the address size and page size, anywhere from direct mapping to four levels of page tables__

#### Demand paging

#### Page fault handler

#### Page replacement algorithms

* __FIFO replacement__
* __LRU replacement (why can't we use this?)__
* __Not frequently used replacement__
* __Clock (second chance) replacement__

#### Locality and the working set

#### Thrashing

#### Resident set

#### Using page fault frequency to manage resident sets
