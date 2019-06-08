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

使用delete breakpoints 断点号 删除断点

  


这里的断点号表示的是第几个断点,刚才执行break 10返回 reakpoint 1 at 0x40050a: file e.c, line 10.

  


中的1表示该断点的标号，因此使用 delete breakpoints 1表示删除第10行所定义的断点

  


clear n表示清除第n行的断点,因此clear 10等同于delete breakpoints 1

  


disable/enable n表示使得编号为n的断点暂时失效或有效

  


可使用info查看断点相关的信息

  


info breakpoints

