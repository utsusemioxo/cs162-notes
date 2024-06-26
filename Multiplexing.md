# Multiplexing
## multiplexing intuition
### multiplexing is an idea of time slicing
![](thread_02.svg)
### multiplexing share all non-CPU resources
![](thread_01.svg)
- All vCPU's share non-CPU resources
	- Memory, I/O Devices
- Each thread can read/write memory
	- Perhaps data of others
	- can overwrite OS? => YES if not protected.
- Unusable? => 实际上早期确实可以用，并且出现了很多问题
	- This approach is used in
		- Very early days of computing
		- Embedded applications
		- MacOS 1-9/Windows 3.1 (switch only with voluntary yield)
			- 在Windows 3.1上面写一个死循环，就会导致所有其他程序都停掉
		- Windows 95-ME (switch with yield or timer)
- IT IS RISKY!!!
> [Yield (multithreading)](https://en.wikipedia.org/wiki/Yield_(multithreading)) 
> In computer science, **yield**（**让步**） is an action that occurs in a computer program during multithreading, of forcing a processor to relinquish control of the current running thread, and sending it to the end of the running queue, of same scheduling priority.
> “强制处理器放弃对于当前正在运行的线程的控制权，并将其发送到相同优先级的运行队列的末尾”

## Risks of Multiplexing without protection
- Operating System must protect itself from user programs
	- Reliability: compromising the operating system generally causes it to crash
	- Security: limit the scope of what threads can do
	- Privacy: limit each thread to the data it is permitted to access
	- Fairness: each thread should be limited to its appropriate share of system resources (CPU time, memory, I/O, etc)
- OS must protect User programs from one another
	- Prevent threads owned by one user from impacting threads owned by another user
	- Example: prevent one user from stealing secret information from another user

## Base and Bond (B&B) protection
a simple protect in early days using Base and Bond register.

![](base_and_bound_01.svg)
We use Base register and Bound register to check if the program address is valid. If it's valid, the code can continue to run.

> How expensive to do this?
> - We can do base and bound check in parallel.

> How to change base and bound?
> - Only If I'm in kernel mode, can I change base and bound. So Only operating system can modify it, so it can protect program.

another implementation look like this:
![](base_and_bound_02.svg) 第二种是一种硬件的relocation，不需要软件来实现地址重定位。
Bound is the size rather an address.


- This is Hardware relocation (The second way)
- Can the program touch OS? NO
- Can it touch other programs? NO

## Another idea: Address Space Translation
### What is address space translation?
>Program operates in an address space that is distinct from the physical memory space of the machine.

![](address_space_translation_01.svg)
### Paged Virtual Address Space
- What if we break the entire virtual address space into equal size chunks (i.e., pages) have a base for each?
- Treat memory as page size frames and put any page into any frame...
![](paged_virtual_address_01.svg)
- Instructions operate on virtual address
	- Instruction address, load/store data address
- Translated to a physical address (or Page Fault) through a Page Table by the hardware
- Any Page of address space can be in any (page sized) frame in memory
	- Or not-present (access generates a page fault)
- Special register holds page table base address (of the process)