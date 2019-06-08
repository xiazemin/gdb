查看调用堆栈

```
(gdb) s
debug (str=0x100000fa8 "123456") at e.c:7
7           printf("debug info :%s\n",str );
(gdb) bt
#0  debug (str=0x100000fa8 "123456") at e.c:7
#1  0x0000000100000f57 in main (argc=1, argv=0x7fff5fbff0b0) at e.c:15

```



