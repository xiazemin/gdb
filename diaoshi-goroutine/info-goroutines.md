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



