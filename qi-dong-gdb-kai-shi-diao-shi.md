1.编译带调试信息的可执行程序：用gcc\(g++\)编译的时候带上-g选项即可

2.启动GDB开始调试

```
（
1
）
gdb
program
       ///
最常用的用
gdb
启动程序，开始调试的方式
```

```
（
2
）
gdb
program core
   ///
用
gdb
查看
core dump
文件，跟踪程序
core
的原因
```

```
（
3
）
gdb
program pid
    ///
用
gdb
调试已经开始运行的程序，指定
pid
即可
```

3.应用程序带命令行参数的情况，可以通过下面三种方法启动：

（1）启动GDB的时候，加上--args选项，然后把应用程序和其命令行参数带在后面，具体格式为：**gdb**--args_program args_

（2）先按1中讲的方法启动GDB,然后在执行run命令的时候，后面加上参数

（3）先按1中讲的方法启动GDB，然后用**set args**_arglist_设置命令行参数，接下来再执行**run**\(**r**\)命令运行程序

4.退出GDB：

（1）**End-of-File\(Ctrl+d\)**

（2）**quit**或者**q**

5.在GDB调试程序的时候执行shell命令：

（1）**shell**_command args_（也可以先执行**shell**命令，GDB会退出到当前shell,执行完_command_后，然后在shell中执行**exit**命令，便可回到GDB）

（2）**make**_make-args_（等同于**shell make**_make-args_）

6.在GDB中获取帮助：

（1）在GDB中执行**help**命令，可以得到如图1所示的帮助信息：



图1 GDB帮助菜单

由图1可以看出，GDB中的命令可以分为八类：别名\(aliases\)、断点\(breakpoints\)、数据\(data\)、文件\(files\)、内部\(internals\)、隐含\(obscure\)、运行\(running\)、栈\(stack\)、状态\(status\)、支持\(support\)、跟踪点\(tracepoints\)和用户自定义\(user-defined\)。

（2）**help**_class-name_：查看该类型的命令的详细帮助说明

（3）**help all**：列出所有命令的详细说明

（4）**help**_command_：列出命令_command_的详细说明

（5）**apropos**_word_：列出与_word_这个词相关的命令的详细说明

（6）**complete**_args_：列出所有以_args_为前辍的命令

7.**info**和**show**：

（1）**info**：用来获取和被调试的应用程序相关的信息

（2）**show**：用来获取GDB本身设置相关的一些信息

