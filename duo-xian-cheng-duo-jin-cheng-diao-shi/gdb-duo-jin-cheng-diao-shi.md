gcc -g mp.c -o mp

其实对多进程调试, 先介绍一个 常用的, 调试正在运行的程序. 首先让 ./mp 跑起来.

换一个更容易区分的名字

$ mv mp mp.xiazemin

./mp.xiazemin

$ps aux \|grep 'mp.xiazemin'

didi             83869   0.0  0.0  2444052    768 s002  S+    7:28下午   0:00.00 grep mp.xiazemin

didi             83713   0.0  0.0  2452220    668 s001  S+    7:27下午   0:00.00 ./mp.xiazemin

再通过 ps -ef 找到需要调试的进程. 复制进程文件描述符pid.

这时候启动gdb.

**attach pid**

gdb就把pid那个进程加载进来了. 加载的进程会阻塞到当前正在运行的地方. 直到使用命令控制. 这个功能还是非常猛的.

最后介绍 进程调试的有关命令\(需要最新的gdb才会支持\). 多进程的调试思路和多线程调试流程很相似.

    (gdb) attach 83869
    Attaching to process 83869
    Can't attach to process 83869: No such process (3)
    (gdb) attach 83713
    Attaching to process 83713
    Reading symbols from /Users/didi/PhpstormProjects/c/c/gdb/mp.xiazemin...warning: `/var/folders/r9/35q9g3d56_d9g0v59w9x2l9w0000gn/T/mp-321cad.o': can't open to read symbols: No such file or directory.
    (no debugging symbols found)...done.
    0x00007fff98b242b2 in __semwait_signal () from /usr/lib/system/libsystem_kernel.dylib
    (gdb) 

GDB可以同时调试多个程序。

只需要设置follow-fork-mode\(默认值：parent\)和detach-on-fork（默认值：on）即可。



   设置方法：set follow-fork-mode \[parent\|child\]   set detach-on-fork \[on\|off\]



   查询正在调试的进程：info inferiors

   切换调试的进程： inferior &lt;infer number&gt;

