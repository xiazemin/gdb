1.**attach**_process-id_/**detach**

（1）**attach**_process-id_:在GDB状态下，开始调试一个正在运行的进程，其进程ID为_process-id_

（2）**detach**:停止调试当前正在调试有进程，与**attach**配对试用

**2.kill**

（1）基本功能：杀掉当前GDB正在调试的应用程序所对应的子进程

（2）如果想不退出GDB而对当前正在调试的应用程序重新编译、链接，可以在GDB中执行**kill**杀掉子进程，等编译、链接完后，再重新执行**run**，GDB便可加载新的可执行程序启动调试

3.多线程程序调试相关：

（1）**thread**_threadno_：切换当前线程到由_threadno_指定的线程

（2）**info threads**：查看GDB当前调试的程序的各个线程的相关信息

（3）**thread apply**\[_threadno_\] \[**all**\]_args_：对指定（或所有）的线程执行由args指定的命令

4.多进程程序调试相关\(fork/vfork\)：

（1）缺省方式：fork/vfork之后，GDB仍然调试父进程，与子进程不相关

（2）**set follow-fork-mode**_mode_：设置GDB行为，_mode_为**parent**时，与缺省情况一样；_mode_为**child**时，fork/vfork之后，GDB进入子进程调试，与父进程不再相关

（3）**show follow-fork-mode**：查看当前GDB多进程跟踪模式的设置

5.**step**&**stepi**

（1）**step**\[_count_\]:如果没有指定_count_,则继续执行程序，直到到达与当前源文件不同的源文件中时停止；如果指定了_count_,则重复行上面的过程_count_次

（2）**stepi**\[_count_\]:如果没有指定_count_,继续执行下一条机器指令，然后停止；如果指定了_count_，则重复上面的过程_count_次

（3）**step**比较常见的应用场景：在函数_func_被调用的某行代码处设置断点，等程序在断点处停下来后，可以用**step**命令进入该函数的实现中，但前提是该函数编译的时候把调试信息也编译进去了，负责**step**会跳过该函数。

6.**next**&**nexti**

（1）**next**\[_count_\]:如果没有指定_count_,单步执行下一行程序；如果指定了_count_，单步执行接下来的_count_行程序

（2）**nexti**\[_count_\]:如果没有指定count,单步执行下一条指令；如果指定了count,单步执行接下来的count条执行

（3）**stepi**和**nexti**的区别：**nexti**在执行某机器指令时，如果该指令是函数调用，那么程序执行直到该函数调用结束时才停止。

7.**continue**\[_ignore-count_\]:唤醒程序，继续运行，至到遇到下一个断点，或者程序结束。如果指定_ignore-count_，那么程序在接下来的运行中，忽略_ignore-count_次断点。

8.**finish**&**return**

（1）**finish**:继续执行程序，直到当前被调用的函数结束，如果该函数有返回值，把返回值也打印到控制台

（2）**return**\[_expression_\]:中止当前函数的调用，如果指定了_expression_，把_expresson_值当做当前函数的返回值；如果没有，直接结束当前函数调用

9.信号的处理

（1）**info signals**&**info handle**：打印所有的信号相关的信息，以及**GDB**缺省的处理方式



（2）**handle**_signal keywords_:设置**GDB**对具体某个信号的处理方式。_signal_可以为信号整数值，也可以为SIGSEGV这样的符号。keywords的取值有：

a.**stop**和**nostop**:**nostop**表示当**GDB**收到指定的信号，不会应用停止程序的执行，只会打印出一条收到信号的消息，因此，**nostop**也暗含了下面的**print**;而**stop**则表示，当**GDB**收到指定的信号，停止应用程序的执行。

b.**print**和**noprint**:**print**表示如果收到指定的信号，打印出一条信息;**noprint**与**print**表示相反的意思

c.**pass**和**nopass**：**pass**表示如果收到指定的信号，把该信号通知给应用程序;**nopass**表示与pass相反的意思

d.**ignore**和**noignore**:**ignore**与**nopass**同义，同理，**noignore**与**pass**同义

