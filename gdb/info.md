\(gdb\) info

"info" must be followed by the name of an info command.

List of info subcommands:

info address -- Describe where symbol SYM is stored

info all-registers -- List of all registers and their contents

info args -- Argument variables of current stack frame

info auto-load -- Print current status of auto-loaded files

info auxv -- Display the inferior's auxiliary vector

info bookmarks -- Status of user-settable bookmarks

info breakpoints -- Status of specified breakpoints \(all user-settable breakpoints if no argument\)

\(gdb\) info  0x100001940

Undefined info command: "0x100001940".  Try "help info".

\(gdb\) info runparent

Undefined info command: "runparent".  Try "help info".

可使用info查看断点相关的信息

info breakpoints

主要看** info args  查看当前函数参数值**

**info locals 看当前函数栈上值信息, info registers 表示查看寄存器值.**

```
(gdb)  info args 
argc = 1
argv = 0x7fff5fbff0b0
(gdb) infor registers
Undefined command: "infor".  Try "help".
(gdb) info registers
rax            0x100000f8e      4294971278
rbx            0x0      0
rcx            0x20000000303    2199023256323
rdx            0x30000000300    3298534884096
rsi            0x12068  73832
rdi            0x100000fa8      4294971304
rbp            0x7fff5fbff090   0x7fff5fbff090
rsp            0x7fff5fbff070   0x7fff5fbff070
r8             0x40     64
---Type <return> to continue, or q <return> to quit---return
r9             0x7fff7ca9a110   140735284879632
r10            0xffffffffffffffff       -1
r11            0x246    582
r12            0x0      0
r13            0x0      0
r14            0x0      0
r15            0x0      0
rip            0x100000f57      0x100000f57 <main+87>
eflags         0x206    [ PF IF ]
---Type <return> to continue, or q <return> to quit---return
cs             0x2b     43
ss             <unavailable>
ds             <unavailable>
es             <unavailable>
fs             0x0      0
gs             0x0      0
(gdb) info locals
i = 1
j = 10
(gdb) 

```



