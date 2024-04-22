# Four Fundamental OS Concepts
## Thread: Execution context
[[Thread]]
- Fully describes program state
- Program Counter, Registers, Execution Flags, Stack
## Address space (with or w/o translation)
[[Address_Space]]
- Sets of memory address accessible to program (for read or write)
- May be distinct from memory space of the physical machine (in which case programs operate in a virtual address space)
## Process: an instance of a running program
[[Process]]
- Protected Address Space + One or more Threads
## Dual mode operation / Protection
[[Dual_Mode_Operation]]
- Only the "system" has the ability to access certain resources
- Combined with translation, isolates programs from each other and the OS from programs