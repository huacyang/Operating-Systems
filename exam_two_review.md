## Memory Management

__Single partition monoprogramming__ allows only a single program to be loaded in memory at any given time.
* The program shares its memory with the OS and nothing else.

__CPU utilization__ is the estimated percentage of time that the CPU is not idle.

The __degree of multiprogramming__ is the number of processes in memory at one time and the candidates for running.

__Absolute code__ uses real memory addresses and expects to be loaded into and run from a specific memory location.

__Position independent code__ uses relative addresses throughout and may be loaded into any memory location.
* Risk: some absolute memory references may have been used.

__Relocation table__ provide empty memory references, where they are filled when a program is loaded.

__Virtual addressing__ (logical) rely on hardware to dynamically transform address references made by the process into actual memory locations.
* Approach: add a __base address__ to each memory reference made by a process.
* A _logical address_ is effectively a combination of a segment identifier and an offset within that segment.

__Base & limit addressing__ ensures that the process is accessing only memory within the process, by checking the virtual address against a _limit_.
* Approach: requires each process to occupy contiguous memory.
* __Far pointer__ is the combined segment value and offset.

In __multiple fixed partitions__, the OS divides memory into a bunch of partitions ahead of time.
* A program is loaded into an available partition that can hold it.
* If no partition is available, the loading will have to be delayed in a queue of requests.
* __Internal fragmentation__ is where unused space within a partition remains unused.
* __External fragmentation__ is where if any partitions have no programs to run, then their memory is always wasted.

In __variable partitions__, the OS creates a partition for a process on demand, using a block of contiguous memory that is big enough to hold it.
* Cons: unallocated regions will develop as processes come and go
* __Memory compaction__ can relocate processes in memory but is a time-consuming approach.

## Page-based Virtual Memory

In __page-based memory management unit__, the physical memory is divided into equal-sized chunks that are multiple of a power of two.
* Logic: use a fixed number of bits to address a byte offset within the chunk.
* The chunks are call __page frames__.

A logical memory address is divided into two components:
* The __page number__ is the high-order bits of the address.
* The __page offset__ is the low-order bits of the address.
* I.E. in a 4KB pages
  - The lower 12 bits (2<sup>12</sup> = 4096) is used to identify the _page offset_
  - The higher 20 bits is used to identify the _page number_

__Page table__ is the data structure used by a virtual memory system in the OS to store the mapping between virtual addresses and physical addresses.
* Each __page table entry__ (PTE) contains a page frame number.
* A __direct mapping paging system__ contains a page table per process.
* The __page table base register__ is used when the OS performs a context switch to run a specific process.
* The __page residence bit__ indicates whether that specific page is mapped onto a page frame.
* A __page fault__ is generated if the page is not mapped onto a page frame.

A __translation lookaside buffer__ (TLB) is used to store recently-accessed page numbers and their corresponding PTEs.
* Method: employs an __address space identifier__ (ASID) to identify the process to whom an entry belongs.
* The __hit ratio__ is the percentage of memory references that can be satisifed from the TLB without resorting to a page table lookup.

A __multilevel page table__ is used to save memory.
* The virtual address is split into three or more parts:
  - The lowest bit still gives the offset within a page and page frame.
  - The highest bit serve as an offset into a top-level __index table__.
  - Each entry contains the base address of a __partial page table__.

The __inverted page table__ is another approach to deal with very large address spaces.
  - Logic: a single table with one entry per page frame.

Features of a __paging system__:
* It does not let the amount of physical memory limit the size of the process.
* It allows the process to feel like it owns the entire address space of the processor.
* It allows memory allocation to be non-contiguous and has no external fragmentation problems, simplifying memory management and obviating any need for compaction.
* It keeps more processes in memory than the sum of their memory needs, so that the CPU utilization can be kept as high as possible.
* It keeps just the parts of a process that are used in memory; the rest may be stored on a disk.

## Demand Paging

__Demand paging__ refers to the technique of allocating and loading memory pages on _demand_.

__Page replacement__ is the process of finding a page to save out onto disk to create a free page frame.
* __First in, first out__ (FIFO)
  - Algorithm: the oldest entry ('bottom' of the stack) is processed first.
* __Least recently used__ (LRU)
  - Algorithm: discards the least recently used item.
* __Least frequently used__ (LFU)
  - Algorithm: drops the item with the lowest reference frequency.
* __Clock algorithm__ (second change algorithm) looks at page frames as a circular list.
  - Algorithm: starts at the current position, looks for page frames whose reference bits are 0 (0 = not been referenced), sets the referenced bits of pages it skipped over to 0.

__Locality__
* Principle: a process tends to reference the same pages over and over before moving on to a few other pages.
* These set of pages is known as the process' __working set__.
  - __Thrashing__ is deployed when not all of the process' working set is in memory: constantly paging to and from the disk.
* The __working set model__ approximates the locality characteristics of a process to try to identify the set of pages that a process needs to have its working set in memory and avoid thrashing.
* __Page faults__ is generated when a process does not have its working set in memory.
  - Page fault = needs more memory
  - No page fault = too much memory.

## Devices

A __block device__ provides structured access to the underlying hardware.
* Devices that can host a file system.
* Provide addressable (block numbers) I/O of fixed, equal-sized chunks of bytes.

The __buffer cache__ is a pool of kernel memory that is allocated to hold frequently used blocks from block devices.

A __character device__ provides unstructured access to the underlying hardware.
* Presents a continuous sequence of bytes.
* I.E. printers, scanners, mice, video frame buffer.

A __network device__ is similar to a concept device, except that instead of sending and receiving a stream of bytes, it sends and receives bytes in chunks (packets).

The __major number__ is an index into either the block or character device table, and allows the kernel to find the set of functions that are associated with the device: the __device driver__.

The __minor number__ identifies which specific instance of a device is being accessed.

## Execution Contexts

Any kernel code is running in one of three contexts:
1. __Interrupt context__: this is invoked by the spontaneous change in the flow of execution when a hardware interrupt takes place.
  - Cannot be blocked because there is no process that requested the action that could be put to sleep and scheduled to wake up when an operation is complete.
2. __User context__: this is the flow of control when a user process invokes a system call.
  - The mode switch to kernel mode, but the context is still that of a user process.
  - The kernel may schedule an I/O operation, putting the process into a _blocked_ state, and context switch to another process.
3. __Kernel context__: the kernel itself has one or more worker threads that is schedules just like any other process.
  - May be blocked.

## Device Drivers

To ensure that the interrupt context does not take too long (due to unable to block), it is split into two parts:
1. The __top half__ of a driver is the interrupt service routine that is registered with the interrupt handler.
  - It tries to do as little as possible -- generally grabbing data, placing it into a __work queue__ (a buffer).
2. The __bottom half__ of a driver is the part that is scheduled by the top half for later execution.
  - It runs in a kernel thread.
  - May perform blocking operations.

A __device status table__ keeps track of devices and the current status of each device.

## Disk Scheduling

Disk scheduling algorithms:
* __First come, first served__ (FCFS) - processes are requested in the order in which they are generated.
* __Elevator algorithm__ (SCAN) - requests are sorted in the order in which they will be encountered as the head moves from the track - to the highest track on the disk, then back to track 0.
* __LOOK__ - reverses the disk head's direction if there are no blocks scheduled beyond the current point.
* __Circular SCAN__ (C-SCAN) algorithm is a SCAN algorithm, but schedules disk requests only when the head is moving in one direction to avoid preferential treatment of blocks in the middle.
* __Circular LOOK__ (C-LOOK) algorithm is like LOOK but schedules requests in only one direction.
* __Shortest Seek Time First__ (SSTF) - the queue of current requests is been sorted by cylinder numbers closest to the current head position and requests made in that order.

## File Systems

__Virtual File System__ is a layer of abstraction, used to support multiple file systems transparently.

Layers of flavors:
* __System call interface__: APIs for user programs.
* __Virtual file system__: maanges the namespace, keeps track of open files, reference counts, file system types, mount points, pathname traversal.
* __File system module__ (driver): understands how the file system is implemented on the disk. Can fetch and store metadata and data for a file, get directory contents, create and delete files and directories.
* __Buffer cache__: no understanding of the file system. Takes read and write requests for blocks or parts of a block and caches frequently used blocks.
* __Device drivers__: the components that actually know how to read and write data to the disk.

VFS crucial components:
* __inode__
  - uniquely identifies a file.
  - In file system, it stores key information about the attributes of a file and the location of its data.
  - In VFS, it contains functional interfaces for creating files, directories, symbolic links, reading and setting attributes (operations not related to file's data).
* __dentry__ (directory entry)
  - an abstract representation of directory contents.
  - Contains the filename as a string, its inode, and a pointer to its parent dentry.
* __file__
  - represents an open file and keeps track of the access mode and reference counts.
* __superblock__
  - contains interfaces to get information about the file system, read/write/delete inodes, and lock the file system.

## Managing Files on Disks

__Contiguous allocation__ - if a file's data requires multiple disk blocks, the blocks would be contiguous.
  - Cons: may lead ot external fragmentation.

__File allocation table__ (FAT)
* A table of block numbers is created.
* The entire chain of blocks can be read fromt he file allocation table.

__Indexed allocation__
* With each file, it is associated with an entire list of blocks that the file uses.
* Not practical: there will ve varying size list (some might be extremely long).

__Combined indexing__
* Uses fixed length structures.

## File System Implementation Case Studies

__Unix File System__ (UFS)
* An implementation of the inode-based just described.
* Uses a linked-list storage system.
* Contains: a superblock, inode blocks, and data blocks.

__Berkeley Fast File System__ (FFS)
1. __Larger clusters__
  - Brings about an eight-fold improvement in contiguous allocation.
  - To avoid too much internal fragmentation from large cluster sizes, the last cluster of a file is a fragment.
2. __Cylinder groups__
  - Multiple regions are created for inodes and other data.
3. __Bitmap allocation__
  - Instead of using a linked list of free blocks, a bitmap is used to keep track of which blocks are in use within each cylinder group.
4. __Prefetch__
  - If two or more sequential blocks are read from a file, FFS assumes that the file access is sequential and will _prefetch_ additional blocks from that file.

__Linux ext2__
  - An adaption of Berkeley's FFS.
  - Cylinder groups --> __block group__: an abstraction of a long linear list of blocks.
  - Ext2 caches all data aggressively in the buffer cache, to improve disk corruption.

__Linux ext3__
  - The __consistent update problem__ is the challenge of performing all updates atomically: either all of them should be applied to the file system or none should be, ensuring that the file system can always remain in a consistent state.
  - __Journaling__ is a method of providing this guarantee of atomicity.
  - The downside of _journaling_ is performance: all disk operations are applied to the journal and then applied again to the file system.

__Full data journaling__
All disk operations are written to the journal first, then applied to the file system.

__Ordered journaling__
* This writes data blocks first, then journals only the metadata, terminates the transaction, and then applies all the metadata that was journaled onto the file system.
* The file system is always kept in a consistant state but in some cases may lead to partially modified files.

__Writeback journaling__
* This does metadata journaling, but without writing the data block first.
* Recently modified files can be corrupted after a crash.
* Its' the fastest of the journaling options and the weakest (due to data corruption).

__Linux ext4__  
1. __Extent-based allocation__
  * Contains the starting cluster number of a group of clusters along with a cluster count.
2. __Delayed allocation of disk space__
  * The blocks are kept in the buffer cache and allocated to physical blocks only when they are ready to be flushed.
  * Increases the chance of allocating contiguous blocks for a file.

__Microsoft NTFS__
* Every structure in the file system appears as a file.
* The __Master File Table__ (MFT) is the structure that keeps track of file records.
  - Structured as a B-tree to facilitate searching for a specific file record.
