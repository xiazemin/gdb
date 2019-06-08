[https://github.com/iovisor/bcc](https://github.com/iovisor/bcc)

BPF 工具箱里的[bcc](https://github.com/iovisor/bcc)工具集有一个对[cachetop.py](https://github.com/iovisor/bcc/blob/master/tools/cachetop.py)的 pull 请求，它通过程序使用 top-like display 显示 page cache 的统计。太好了 ！然而，当我测试它时，遇到了段错误︰

\# ./cachetop.py Segmentation fault

| 12 | \# ./cachetop.pySegmentationfault |
| :--- | :--- |


注意它说的是“段错误”，不是“段错误（核心已转储）”。我想要一个核心转储文件用来调试。（核心转储文件是进程内存的拷贝 – 这个名字来源于磁芯存储器时代 – 可用调试器分析）

分析核心转储文件是一种方法，但不是调试这个问题的唯一方法。我可以在 gdb 中运行此程序，来检查这个问题。我也可以在段错误发生时，用外部追踪器去抓数据和栈帧。我们从核心转储文件入手。

## 2. 解决核心转储问题

我检查一下核心转储的设置：

\# ulimit -c 0 \# cat /proc/sys/kernel/core\_patter

| 1234 | \# ulimit -c0\# cat /proc/sys/kernel/core\_patterncore |
| :--- | :--- |


ulimit -c 显示核心转储文件大小的最大值，这里是零：禁止核心转储（对于本进程和它的子进程）。

/proc/…/core\_pattern 仅仅被设为 “core”，表示会在当前目录下生成一个文件名为 “core” 的 核心转储文件。目前这样就行了，但是我要演示如何把它设置为全局位置。

\# ulimit -c unlimited \# mkdir /var/cores \# echo "/var/cores/core.%e.%p" 

&gt;

 /proc/sys/kernel/core\_pattern

| 123 | \# ulimit -c unlimited\# mkdir /var/cores\# echo "/var/cores/core.%e.%p" &gt; /proc/sys/kernel/core\_pattern |
| :--- | :--- |


你可以进一步定制 core\_pattern；例如，%h 为主机名，%t 为转储的时间。这些选项被写在 Linux 内核源码 Documentation/sysctl/[kernel.txt](https://www.kernel.org/doc/Documentation/sysctl/kernel.txt)中。

要使 core\_pattern 保持不变，重启之后仍然有效，你可以通过设置 /etc/sysctl.conf 里的 “kernel.core\_pattern” 实现。

再来一次：

\# ./cachetop.py Segmentation fault \(core dumped\) \# ls -lh /var/cores total 19M -rw------- 1 root root 20M Aug 7 22:15 core.python.30520 \# file /var/cores/core.python.30520 /var/cores/core.python.30520: ELF 64-bit LSB core file x86-64, version 1 \(SYSV\), SVR4-style, from 'python ./cachetop.py'

| 1234567 | \# ./cachetop.pySegmentationfault\(coredumped\)\# ls -lh /var/corestotal19M-rw-------1rootroot20MAug722:15core.python.30520\# file /var/cores/core.python.30520/var/cores/core.python.30520:ELF64-bitLSBcorefilex86-64,version1\(SYSV\),SVR4-style,from'python ./cachetop.py' |
| :--- | :--- |


好多了：我们有了自己的核心转储文件。

