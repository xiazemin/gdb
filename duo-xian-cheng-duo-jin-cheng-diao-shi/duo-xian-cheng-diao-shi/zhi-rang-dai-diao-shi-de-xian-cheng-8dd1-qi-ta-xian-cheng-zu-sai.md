**set scheduler-locking on**

开始多线程单独调试. 不用了 设置

**set scheduler-locking off **

关闭. 又会回到你调试这个, 其它线程不阻塞.

```
(gdb) set scheduler-locking on
Target 'native' cannot support this command.
(gdb) info threads
  Id   Target Id         Frame 
  1    Thread 0x1313 of process 68108 0x0000000100000dde in main () at mp.cpp:49
  2    Thread 0x1513 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
* 3    Thread 0x1603 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
  4    Thread 0x1703 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23

```



