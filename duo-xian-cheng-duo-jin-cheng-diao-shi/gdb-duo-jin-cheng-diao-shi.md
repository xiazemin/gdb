gcc -g mp.c -o mp

其实对多进程调试, 先介绍一个 常用的, 调试正在运行的程序. 首先让 ./mp 跑起来.

换一个更容易区分的名字

$ mv mp mp.xiazemin

./mp.xiazemin

$ps aux \|grep 'mp.xiazemin'

didi             83869   0.0  0.0  2444052    768 s002  S+    7:28下午   0:00.00 grep mp.xiazemin

didi             83713   0.0  0.0  2452220    668 s001  S+    7:27下午   0:00.00 ./mp.xiazemin

再通过 ps -ef 找到需要调试的进程. 复制进程文件描述符pid.

这时候启动gdb.

**attach pid**

gdb就把pid那个进程加载进来了. 加载的进程会阻塞到当前正在运行的地方. 直到使用命令控制. 这个功能还是非常猛的.

最后介绍 进程调试的有关命令\(需要最新的gdb才会支持\). 多进程的调试思路和多线程调试流程很相似.

