# Thread
![](thread_01.svg)
## What is thread?
**Thread**: Single unique execution context
- Program Counter, Registers, Execution Flags, Stack, Memory State
- A thread is *executing* on a processor (core) when it is *resident* in the processor registers
- Resident means: Registers hold the root state (context) of the thread:
	- Including program counter (PC) register & currently executing instruction
		- PC points at next instruction in memory
		- Instructions stored in memory
	- Including intermediate values for ongoing computations
		- Can include actual values (like integers) or pointers to values in memory
	- Stack pointer holds the address of the top of stack (which is in memory)
	- The rest is "in memory"
- A thread is *suspended* (not executing) when its state *is not* loaded (resident) into the processor
	- Processor state pointing at some other thread
	- Program counter register *is not* pointing at next instruction from this thread
	- Often: a copy of the last value for each register stored in memory

## The Illusion of Multiple Processors
- Assume a single processor (core). How to we provide the illusion of multiple processors?
	- Multiplex in time! (or [[Multiplexing]])（时间分割）
	- Threads are *virtual cores*
	- Contents of virtual core (thread): 
		- Program counter, stack pointer
		- Registers
	- Where is the thread?
		- On the real (physical) core, or
		- Saved in chunk of memory - called the *Thread Control Block (TCB)* 
> Is the state of one thread is preserved in registers while the other one runs?
> - Usually what happens is that the state of the registers are stored in the Thread Control Block (TCB)

> Does all threads getting equal amount of time?
> - who gets how much time is decide on the resource control and scheduling

> "virtual core"是否意味着程序并不知道自己并没有完整的CPU的控制权？不是，而是意味着程序不需要做修改就可以好像拥有完整的控制权一样在CPU上面运行。但是应该总有办法知道自己其实只是拥有virtual的CPU

- Consider:
	- At T1: vCPU1 on real core, vCPU2 in memory
	- At T2: vCPU2 on real core, vCPU1 in memory
- What happened?
	- OS Ran [how?]
	- Saved PC, SP, ... in vCPU1's thread control block (memory)
	- Loaded PC, SP, ... from vCPU2's TCB, jumped to PC
- What triggered this switch?
	- Timer, voluntary yield, I/O, other things we will discuss
![](thread_02.svg)
