# Atomic Operation
**Atomic Operation**: an operation that always runs to completion or not at all.
>*“全部或没有”*

原子操作没有中间状态，要么完成，要么一点没做。

- On most machines, memory references and assignments (i.e. loads and stores) of words are atomic
- Many instructions are not atomic
	- Double-precision floating point store often not atomic
	- VAX and IBM 360 had an instruction to copy a whole array
