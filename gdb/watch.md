watch可设置观察点\(watchpoint\)。使用观察点可以使得当某表达式的值发生变化时,程序暂停执行。

执行该命令前，必须保证程序已经运行

```
(gdb) watch j
Hardware watchpoint 4: j
(gdb) c
Continuing.
now a=30
debug info :123456

Breakpoint 2, main (argc=1, argv=0x7fff5fbff0b0) at e.c:14
14              printf("now a=%d\n", j);
(gdb) c
Continuing.
now a=35
debug info :123456

Breakpoint 2, main (argc=1, argv=0x7fff5fbff0b0) at e.c:14
14              printf("now a=%d\n", j);
(gdb) c
Continuing.
now a=40
debug info :123456

Breakpoint 2, main (argc=1, argv=0x7fff5fbff0b0) at e.c:14
14              printf("now a=%d\n", j);
(gdb) 

```



