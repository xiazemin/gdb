gcc -g mp.c -o mp

其实对多进程调试, 先介绍一个 常用的, 调试正在运行的程序. 首先让 ./mp 跑起来.

换一个更容易区分的名字

$ mv mp mp.xiazemin

./mp.xiazemin



$ps aux \|grep 'mp.xiazemin'

didi             83869   0.0  0.0  2444052    768 s002  S+    7:28下午   0:00.00 grep mp.xiazemin

didi             83713   0.0  0.0  2452220    668 s001  S+    7:27下午   0:00.00 ./mp.xiazemin

