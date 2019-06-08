step及next命令

step可使得程序逐条执行,即执行完一条语句然后在吓一跳语句前停下来,等待用户的命令

一般使用step命令是，可使用display或者watch命令查看变量的变化,从而判断程序行为是否符合要求

当下一条指令为函数时,s进入函数内部,在其第一条语句前停下来

step n,next n 表示连续但不执行n条指令,如果期间遇到断点,则停下来

```
(gdb) info breakpoint
No breakpoints or watchpoints.
(gdb) b 14
Breakpoint 2 at 0x100000f3e: file e.c, line 14.
(gdb) r
Starting program: /Users/didi/PhpstormProjects/c/c/gdb/e 

Breakpoint 2, main (argc=1, argv=0x7fff5fbff0b0) at e.c:14
14              printf("now a=%d\n", j);
(gdb) s
now a=5
15              debug("123456");
(gdb) s
debug (str=0x100000fa8 "123456") at e.c:7
7           printf("debug info :%s\n",str );
(gdb) s
debug info :123456
8       }
(gdb) s
main (argc=1, argv=0x7fff5fbff0b0) at e.c:12
12          for(i=0;i<10;i++){
(gdb) n
13              j+=5;
(gdb) n

Breakpoint 2, main (argc=1, argv=0x7fff5fbff0b0) at e.c:14
14              printf("now a=%d\n", j);
(gdb) n
now a=10
15              debug("123456");
(gdb) n
debug info :123456
12          for(i=0;i<10;i++){
(gdb) c
Continuing.

Breakpoint 2, main (argc=1, argv=0x7fff5fbff0b0) at e.c:14
14              printf("now a=%d\n", j);
(gdb) 

```



