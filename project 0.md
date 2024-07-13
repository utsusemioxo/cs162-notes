# pregame project

## do-nothing crash
```txt
workspace@e7d5c36a4d10 [13:44:40] userprog $ pintos-test do-nothing
pintos -v -k -T 60 --qemu  --filesys-size=2 -p tests/userprog/do-nothing -a do-nothing -- -q   -f run do-nothing < /dev/null 2> tests/userprog/do-nothing.errors > tests/userprog/do-nothing.output
perl -I../.. ../../tests/userprog/do-nothing.ck tests/userprog/do-nothing tests/userprog/do-nothing.result
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = (unset),
        LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
FAIL tests/userprog/do-nothing
Test output failed to match any acceptable form.

Acceptable output:
  do-nothing: exit(162)
Differences in `diff -u' format:
- do-nothing: exit(162)
+ Page fault at 0xc0000008: rights violation error reading page in user context.
+ do-nothing: dying due to interrupt 0x0e (#PF Page-Fault Exception).
+ Interrupt 0x0e (#PF Page-Fault Exception) at eip=0x8048915
+  cr2=c0000008 error=00000005
+  eax=00000000 ebx=00000000 ecx=00000000 edx=00000000
+  esi=00000000 edi=00000000 esp=bfffffe4 ebp=bffffffc
+  cs=001b ds=0023 es=0023 ss=0023
workspace@e7d5c36a4d10 [13:44:45] userprog $ pintos-test do-nothing
pintos -v -k -T 60 --qemu  --filesys-size=2 -p tests/userprog/do-nothing -a do-nothing -- -q   -f run do-nothing < /dev/null 2> tests/userprog/do-nothing.errors > tests/userprog/do-nothing.output
perl -I../.. ../../tests/userprog/do-nothing.ck tests/userprog/do-nothing tests/userprog/do-nothing.result
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = (unset),
        LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to the standard locale ("C").
FAIL tests/userprog/do-nothing
Test output failed to match any acceptable form.

Acceptable output:
  do-nothing: exit(162)
Differences in `diff -u' format:
- do-nothing: exit(162)
+ Page fault at 0xc0000008: rights violation error reading page in user context.
+ do-nothing: dying due to interrupt 0x0e (#PF Page-Fault Exception).
+ Interrupt 0x0e (#PF Page-Fault Exception) at eip=0x8048915
+  cr2=c0000008 error=00000005
+  eax=00000000 ebx=00000000 ecx=00000000 edx=00000000
+  esi=00000000 edi=00000000 esp=bfffffe4 ebp=bffffffc
+  cs=001b ds=0023 es=0023 ss=0023
```
- `ip`: **instruction pointer register**,  32位是`eip`, 64位是`rip`。
- `bp`: General Purpose Register之一，存储base address of stack。

### 1.
What virtual address did the program try to access from userspace that caused it to crash? Why is the program not allowed to access this memory address at this point?
	a. 程序想要access的虚拟地址是`0xc0000008`
	b. 为什么会crash，因为这个地址是kernel virtual memory，不能直接access
	> [See Pintos Virtual-memory doc](https://cs162.org/static/proj/pintos-docs/docs/userprog/virtual-memory/)User virtual memory ranges from virtual address `0` up to `PHYS_BASE`, which is defined in `threads/vaddr.h` and defaults to `0xc0000000` (3 GB). Kernel virtual memory occupies the rest of the virtual address space, from `PHYS_BASE` up to 4 GB.
### 2.
What is the virtual address of the instruction that resulted in the crash? 
`0xc0000008` which is in kernel virtual memory.
### 3.
To investigate, disassemble the `do-nothing` binary using `i386-objdump` (you used this tool in Homework 0). What is the name of the function the program was in when it crashed? Copy the disassembled code for that function onto Gradescope, and identify the instruction at which the program crashed.
```txt
int main(int, char*[]);
void _start(int argc, char* argv[]);

void _start(int argc, char* argv[]) { exit(main(argc, argv)); }
 804890f:	55                   	push   %ebp
 8048910:	89 e5                	mov    %esp,%ebp
 8048912:	83 ec 18             	sub    $0x18,%esp
 8048915:	8b 45 0c             	mov    0xc(%ebp),%eax
 8048918:	89 44 24 04          	mov    %eax,0x4(%esp)
 804891c:	8b 45 08             	mov    0x8(%ebp),%eax
 804891f:	89 04 24             	mov    %eax,(%esp)
 8048922:	e8 6d f7 ff ff       	call   8048094 <main>
 8048927:	89 04 24             	mov    %eax,(%esp)
 804892a:	e8 d4 22 00 00       	call   804ac03 <exit>
```

the faulting instruction is 
```txt
8048915:	8b 45 0c             	mov    0xc(%ebp),%eax
```
 这一行x86汇编含义是：把ebp寄存器的值加上0xc之后存到eax寄存器
 0xc0000008=ebp+0xc=0xbffffffc+0xc
 

### 4.
Find the C code for the function you identified above (_Hint: it was executed in userspace, so it’s either in `do-nothing.c` or one of the files in `proj-pregame/src/lib` or `proj-pregame/src/lib/user`_), and copy it onto Gradescope. For each instruction in the disassembled function in #3, explain in a few words why it’s necessary and/or what it’s trying to do. _Hint: read about [80x86 calling convention](https://cs162.org/static/proj/pintos-docs/docs/userprog/calling-convention)._
从上面其实可以看到了，crash在
```C
void _start(int argc, char* argv[]) { exit(main(argc, argv)); }
```
这个函数里面。
路径：/home/workspace/code/personal/proj-pregame/src/lib/user/entry.c

### 5.
Why did the instruction you identified in #3 try to access memory at the virtual address you identified in #1? Please provide a high-level explanation, rather than simply mentioning register values.