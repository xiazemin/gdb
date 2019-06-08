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

**info threads 查看所有运行的线程信息. \*表示当前调试的线程.**

**后面 l \_run 表示查看 \_run附近代码. **当然还有 **l 16** 查看16行附近文件内容.

gdb多线程切换 测试如下

