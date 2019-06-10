```
(gdb) info goroutine
  1 waiting  runtime.gopark
  2 waiting  runtime.gopark
  3 waiting  runtime.gopark
  4 waiting  runtime.gopark
* 5 running  runtime.systemstack_switch
  6 runnable main.main.func1
* 7 running  runtime.systemstack_switch
  8 runnable main.main.func1
  9 runnable main.main.func1
* 10 running  runtime.systemstack_switch
```

```
(gdb) goroutine 11 bt
#0  main.main.func1 (pairChan=0xc420020180, &wg=0xc420016480, val=2) at /Users/didi/goLang/src/go_http/main.go:44
#1  0x00000000010584b1 in runtime.goexit () at /usr/local/go/src/runtime/asm_amd64.s:2337
#2  0x000000c420020180 in ?? ()
#3  0x000000c420016480 in ?? ()
#4  0x0000000000000002 in ?? ()
#5  0x0000000000000000 in ?? ()
```

处理 goroutines 往往不稳定；我遇到过执行简单命令产生错误的情况。现阶段你应该做好处理类似问题的准备。

```
(gdb)  info args 
pairChan = 0xc420020180
&wg = 0xc420016480
val = 0
(gdb) 

```

我们做的第一件事就是列出所有正在运行的 goroutine，并确定我们正在处理的那一个。然后我们可以看到一些回溯，并发送任何调试命令到 goroutine。这个回溯和列表清单并不太准确，如何让回溯更准确，goroutine 上的

**info args**

显示了我们的局部变量，以及主函数中的可用变量，goroutine 函数之外的使用前缀

**&**

。

