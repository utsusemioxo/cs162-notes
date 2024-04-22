# Dual Mode Operation

## introduction
- Hardware provides at least two modes:
	1. Kernel Mode (or "supervisor" / "protected" mode)
	2. User Mode
- Certain operations are **prohibited** when running in user mode
	- e.g. Changing the page table pointer
- Carefully controlled transitions between user mode and kernel mode
	- System calls, interrupts, exceptions
	
![](dual_mode_operation_01.jpeg)
	
	Having dual mode operation allow us to protect hardware.

## UNIX OS Structure
![](unix_os_structure.svg)

- **Hardware** provides at least two modes:
	- "Kernel" mode (or "supervisor" or "protected")
	- "User" mode: Normal programs executed
- What is needed in the hardware to support "dual mode" operation?
	- A bit of state (user/system mode bit)
	- Certain operations / actions only permitted in system/kernel mode
		- In user mode they fail or trap (e.g. setting base and bound registers)
	- User --> Kernel transition *sets* system mode AND saves the user PC
		- Operating system code carefully puts aside user state then performs the necessary operations
	- Kernel --> User transition *clears* system mode AND restores appropriate user PC
		- return-from-interrupt