编译命令

```
gcc -Wall -g -o dasheng.
out
 dasheng.c -lpthread
```

```
(gdb) b 40
Breakpoint 1 at 0x100000da3: file mp.cpp, line 40.
(gdb) r
Starting program: /Users/didi/PhpstormProjects/c/c/gdb/mp mp
main beign
[New Thread 0x1413 of process 63067]

Thread 1 hit Breakpoint 1, main () at mp.cpp:40
40              if(rt < 0) {
(gdb) info threads
  Id   Target Id         Frame 
* 1    Thread 0x1203 of process 63067 main () at mp.cpp:40
(gdb) c
Continuing.
n=1, i=0
n=1, piyo = 10, _old=1
[New Thread 0x150b of process 63067]

Thread 1 hit Breakpoint 1, main () at mp.cpp:40
40              if(rt < 0) {
(gdb) info threads
  Id   Target Id         Frame 
* 1    Thread 0x1203 of process 63067 main () at mp.cpp:40
(gdb) c
Continuing.
n=2, i=0
n=2, piyo = 10, _old=2
n=2, i=1
n=2, piyo = 10, _old=3
[New Thread 0x141f of process 63067]

Thread 1 hit Breakpoint 1, main () at mp.cpp:40
40              if(rt < 0) {
(gdb) info threads
  Id   Target Id         Frame 
* 1    Thread 0x1203 of process 63067 main () at mp.cpp:40
(gdb) info breakpoint
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000000100000da3 in main() at mp.cpp:40
        breakpoint already hit 3 times
(gdb) c
Continuing.
end
[Inferior 1 (process 63067) exited normally]
(gdb)
```

**info threads 查看所有运行的线程信息. \*表示当前调试的线程.**

**后面 l \_run 表示查看 \_run附近代码. **当然还有 **l 16** 查看16行附近文件内容.

gdb多线程切换 测试如下

```
(gdb) b 23
Breakpoint 1 at 0x100000e89: file mp.cpp, line 23.
(gdb) r
Starting program: /Users/didi/PhpstormProjects/c/c/gdb/mp 
main beign
n=1, i=0
n=2, i=0
n=3, i=0
[New Thread 0x1513 of process 68108]
[New Thread 0x1603 of process 68108]
[New Thread 0x1703 of process 68108]
[Switching to Thread 0x1513 of process 68108]

Thread 2 hit Breakpoint 1, _run (arg=0x7fff5fbff068) at mp.cpp:23
23              printf("n=%d, piyo = %d, _old=%d\n", n, piyo, _old);
(gdb) info threads
  Id   Target Id         Frame 
  1    Thread 0x1313 of process 68108 0x0000000100000dde in main () at mp.cpp:49
* 2    Thread 0x1513 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
  3    Thread 0x1603 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
  4    Thread 0x1703 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
(gdb) thread 3
[Switching to thread 3 (Thread 0x1603 of process 68108)]
#0  _run (arg=0x7fff5fbff068) at mp.cpp:23
23              printf("n=%d, piyo = %d, _old=%d\n", n, piyo, _old);
(gdb) info threadas
Undefined info command: "threadas".  Try "help info".
(gdb) info threads
  Id   Target Id         Frame 
  1    Thread 0x1313 of process 68108 0x0000000100000dde in main () at mp.cpp:49
  2    Thread 0x1513 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
* 3    Thread 0x1603 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
  4    Thread 0x1703 of process 68108 _run (arg=0x7fff5fbff068) at mp.cpp:23
(gdb)
```

thread 3表示切换到第三个线程, info threads 第一列id 就是 thread 切换的id.

上面测试线程 就算你切换到 thread 3. 其它线程还是在跑的. 我们用下面命令 只让待调试的线程跑. 其它线程阻塞.



