# Definition
 A virtual machine (VM) is a software emulation of a **physical computer**. It runs an operating system (OS) on top of a host machine's OS, using a layer of abstraction to provide a self-contained, isolated environment for the guest OS and its applications.
## System virtual machines
>"An efficient, isolated duplicate of a real computer machine."

System virtual machines (also called full virtualization VMs) provide a substitute for a real machine. They provide the functionality needed to execute entire operating systems. 
A system virtual machine is a type of virtual machine that is created and managed by a hypervisor. The hypervisor is the software or hardware layer that enables the creation and operation of virtual machines.
A **hypervisor** uses native execution to share and manage hardware, allowing for multiple environments that are isolated from one another yes exist on the same physical machine. Modern hypervisors use hardware-assisted virtualization, with virtualization-specific hardware features on the host CPUs providing assistance to hypervisors.
## Process virtual machines
>A process VM, sometimes called an application virtual machine, or Managed Runtime Environment (MRE), runs as a normal application inside a host OS and supports a single process. It is created when that process is started and destroyed when it exits. Its purpose is to provide a platform-independent programming environment that abstracts away details of the underlying hardware or operating system and allows a program to execute in the same way on any platform.

examples:
- Java virtual machine --> for Java programming language.
- Common Language Runtime --> which .NET Framework runs on.

## VM Examples
### VM and hypervisor
![[hypervisor_example.png]]
One host machine, with three guest virtual machines runs on it with hypervisor.

### Containers virtualize the OS
- Roots in OS developments to provide protected systems abstraction, not just application abstraction
	- User-level file system (route syscalls to user process)
	- Cgroups - predictable, bounded resources (CPU, Mem, BW)