\(gdb\) disp i

No symbol "i" in current context.

\(gdb\) disp kReadEvent

1: kReadEvent = 1

display命令

查看参数的值

\(gdb\) display j

1: j = 5

\(gdb\)

```
(gdb) display j
1: j = 5
(gdb) display j*J
No symbol "J" in current context.
(gdb) display j*j
2: j*j = 25
(gdb) c
Continuing.
now a=10

Breakpoint 3, main (argc=2, argv=0x7fff5fbff0a8) at e.c:13
13              j+=5;
1: j = 10
2: j*j = 100
```

也可以使用disable,enable,delete,info命令修改及查看其状态,用法与对断点的一样

```
(gdb) info display
Auto-display expressions now in effect:
Num Enb Expression
1:   y  j
2:   y  j*j
(gdb) 

```



