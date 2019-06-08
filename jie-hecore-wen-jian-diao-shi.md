使用内核转储（core）的最大好处就是：它能保存问题发生时的状态，只要有问题发生时程序的可执行文件和内核转储，就可以知道进程当时的状态。

* 启用内核转储功能

  ```
  $
  ulimit
   -c

  0

  ```

  `-c`选项表示内核转储文件的大小限制，0表示内核转储未打开。按照以下命令开启:

  ```
  $
  ulimit
   -c unlimited
  $
  ulimit
   -c

  unlimited

  ```

  `ulimit -c unlimited` 设置core文件大小不限制，这样设置是一次性的，会话结束就恢复原样  
   在用户的`~/.bash_profile`里加上`ulimit -c unlimited`来让用户开启内核转储功能  
   在`/etc/profile`里加上`ulimit -c unlimited`来让所有用户开启内核转储功能（?）

* core文件目录和命名规则  
   在`/proc/sys/kernel/core_pattern`可以设置格式化的core文件保存位置和文件名。  
   比如`core-%e-%p-%t`表示在当前目录生成"core-命令-pid-时间戳"为文件名的core文件。  
   比如`/cfg/core-%e-%p-%t`表示在/cfg下生成"core-命令-pid-时间戳"为文件名的core文件

* 参数列表

  | 符号 | 意义 |
  | :--- | :--- |
  | %p | insert pid into filename 添加 pid |
  | %u | insert current uid into filename 添加当前 uid |
  | %g | insert current gid into filename 添加当前 gid |
  | %s | insert signal that caused the coredump into the filename 添加导致产生 core 的信号 |
  | %t | insert UNIX time that the coredump occurred into filename 添加 core 文件生成时的 unix 时间戳 |
  | %h | insert hostname where the coredump happened into filename 添加主机名 |
  | %e | insert coredumping executable name into filename 添加命令名 |

---

###### 多进程调试

gdb的调试默认是调试父进程的，但我们可以通过设置来选择调试哪个进程。

```
(gdb) 
set
 follow-fork-mode parent/child

```

如果选择了parent，这个时候就是进行gdb调试父进程。  
 如果选择了child，这个时候就是进行gdb调试子进程。  
 注意，在调试的过程中更改mode是没有用的，这种设置只对下一次fork后起作用。

  




