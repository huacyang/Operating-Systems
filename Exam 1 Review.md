Introduction
===

* __Multiprogramming__ : keep several programs in memory at once and switch between them.

* __System Call__ : uses the `trap` mechanism to switch control to operating system code running in kernel mode.

* __Batch Systems__ : operating systems that would run a program to completion and then load and run the next program.

* __Device Drivers__

	* __Character Devices__ : any device whose data that can be thought of as a byte stream. This includes keyboard input, mouse movements, printer output, camera inputs, etc.
	* __Block Devices__ : any device that has persistent storage that is randomly addressable and read or written in fixed-size chunks (blocks). These devices include disks and flash memory. Because the data is persistent and addressable, it can be cached by the operating system so that future requests for cached content may be satisfied from system memory instead of accesssing the device again. This cache of frequently-used blocks of data is called the __buffer cache__. Basically, any device that can be used to hold a file system is a block device. 
	* __Network Devices__ : packet-based communications networks.
	
* __Preemption__

* Operating System: What does it do?

* __Mechanism__ versus __Policy__

Booting
===

