\(gdb\) b runparent

Breakpoint 1 at 0x100001940: file fork\_signal.c, line 192.

break location:在location位置设置断点,改位置可以为某一行,某函数名或者其它结构的地址

GDB会在执行该位置的代码之前停下来

\(gdb\) b 10

Breakpoint 1 at 0x100000f26: file e.c, line 10.

\(gdb\) r

Starting program: /Users/didi/PhpstormProjects/c/c/gdb/e e

Unable to find Mach task port for process-id 90315: \(os/kern\) failure \(0x5\).

 \(please check gdb is codesigned - see taskgated\(8\)\)

\(gdb\) 



