# Process
## What is Process?
- Process: execution environment with Restricted Rights
	- (Protected) Address Space with One or More Threads
	- Owns memory (address space)
	- Owns file descriptors, file system context, ...
	- Encapsulate one or more threads sharing process resources
- Application program executes as a process
	- Complex applications can fork/exec child process (later!)
- Why processes?
	- Protected from each other!
	- OS Protected from them
	- Process provides memory protection
	- Thread more efficient than processes for parallelism (later)
- Fundamental tradeoff between protection and efficiency
	- Communication easier *within* a process
	- Communication harder *between* processes
## Single and Multithreaded Processes
![[single_multi_threaded_processes.png]]
- Threads encapsulate **concurrency** --> "active" component
- Address spaces encapsulate **protection** --> "Passive" part
	- Keeps buggy program from trashing the system
- Why have multiple threads per address space?

> Does each thread has its own 0000... and FFFF...?
> - Each thread shared same address space, but each process has it's own 0000..., that's why thread can collaborate with each other so easily, 'cause they all share the same address space. They could screw each other up, but then that's a bug.

> One advantage of having multiple threads in the same process is they can pass structures between each other, and each thread can use the same structure as the other threads, because all of the addresses are shared. Every one of the threads can address the same structure by the same name.


## Kernel code/data in process Virtual Address Space?
