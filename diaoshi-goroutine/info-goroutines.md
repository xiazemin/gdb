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



