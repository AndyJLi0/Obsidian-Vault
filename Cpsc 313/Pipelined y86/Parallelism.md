Our pipeline is very much *simplified*. 
- Not all ALU operations take a single cycle
	- Division and modulo are particularly difficult
- Multiple Functional Units
	- More than one add/multiplier/divider/whatever
- Super Scalar
	- Potentially we can issue more than one instruction per cycle?
- Out-Of-Order Execution and other tricks. (We can utilize these cool tricks to do work or make it so our memory reads look shorter than they actually are)
	- Reorder instructions so they execute faster
	- Move memory instructions early or dependent instructions later
	- Dynamic register renaming on the fly to avoid "output" dependencies

## Limits to the pipeline
- Clock frequency (want as high as possible) (cycles per unit time)
	- results in more heat, clock skew, pipeline depth.
- Cycles per instruction (want as low as possible.
## Some Other Kinds of Parallelism
- **Multitasking** (multi-progamming): Basically an illusion.
	- Software makes it look like many things are happening at once, but quite likely the operating system is simply rapidly switching among programs.
- **Multiprocessing**: Not an illusion.
	- Multiple cores, each with their own caches and pipeline. However, *multiple cores have to use **a single** memory storage.* 
---
- Parallel runtimes that can leverage multiprocessing or other forms of parallelism!
	- **Multithreading** (software), using functional programming languages make threading very easy since objects are not mutable.
	- **Hyperthreading (hardware)**
		- Memory latency problem. Time consuming, but we want processor to stay busy.
		- We run 2 threads on the *same* core. This means each thread needs its own *register file*. However, some things are *shared*, such as the ALU. Each cycle, we can decide which one goes to fetch.
		- If thread 1 has a memory read, we can trade off thread 1 and run thread 2 while thread 1 is busy. We can decide which Hyper thread to bring in on each cycle! 