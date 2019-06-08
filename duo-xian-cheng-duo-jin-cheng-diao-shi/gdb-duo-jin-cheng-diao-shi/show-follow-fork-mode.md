```
(gdb) show follow-fork-mode
Debugger response to a program call of fork or vfork is "parent".
(gdb) 

```

gdb调试多进程的命令：

set follow-fork-mode mode设置调试器的模式

mode参数可以是

parent fork之后调试原进程，子进程不受影响，这是缺省的方式

child fork之后调试新的进程，父进程不受影响。

show follow-fork-mode 显示当前调试器的模式

set detach-on-fork mode 设置gdb在fork之后是否detach进程中的其中一个，或者继续保留控制这两个进程

on 子进程\(或者父进程，依赖于follow-fork-mode的值\)会detach然后独立运行，这是缺省的mode

off 两个进程都受gdb控制，一个进程（子进程或父进程，依赖于follow-fork-mode）被调试，另外一个进程被挂起

infoinferiors 显示所有进程

inferiors processid 切换进程

detach inferiors processiddetach 一个由指定的进程，然后从fork 列表里删除。这个进程会被

允许继续独立运行。

kill inferiors processid 杀死一个由指定的进程，然后从fork 列表里删除。

catch fork 让程序在fork，vfork或者exec调用的时候中断

  


