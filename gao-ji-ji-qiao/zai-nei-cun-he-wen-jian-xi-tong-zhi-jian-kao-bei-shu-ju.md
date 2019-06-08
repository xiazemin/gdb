将内存数据拷贝到文件里

1. ```
   dump binary value file_name variable_name
   dump binary memory file_name begin_addr end_addr

   ```
2. 改变内存数据

   使用set命令

### 执行gdb脚本 {#853df4}

常用的gdb操作，比如打断点等可以放在一个gdb脚本里，然后使用时导入即可。例如:

```
b main.cpp:15
b test.cpp:18

```

gdb运行时，使用source命令即可导入

```
(gdb) source /path/to/breakpoints.txt

```

或gdb运行时导入

```
gdb -x /path/to/breakpoints.txt prog

```

对于每次gdb运行都要调用的脚本，比如设置字符集等，可以放在~/.gdbinit初始文件里，这样每次gdb启动时都会自动调用。

### 输出到文件 {#de2bb9}

可以通过`set logging on`将命令的输出保存到默认的gdb.txt文件中。当然也可以通过`set logging file my_log.txt`来设置输出文件的路径。

### 执行命令并退出 {#69fa7c}

有时候需要gdb执行若干条命令后就立即退出，而不是进入交互界面，这时可以使用`-batch`选项。

```
gdb -ex "set pagination 0" -ex "thread apply all bt" -batch -p $pid

```

上面的命令打印$pid进程所有线程的堆栈并退出。

### 自定义命令 {#ad6526}

参考[gdb/Define](https://sourceware.org/gdb/onlinedocs/gdb/Define.html)，可以在gdb中自定义命令，比如：

```
(gdb) define hello
(gdb) print "welcome"
(gdb) print "hello $arg0"
(gdb) end

```

然后如此调用

```
(gdb) hello world

```

即可输出

```
(gdb) $1 = "welcome"
(gdb) $2 = "hello world"

```

### 条件断点 {#72083b}

在条件断点里可以调用标准库的函数，比如下面这个:

```
# 如果strA == strB，则在断点处暂停
(gdb) b main.cpp:255 if strcmp(strA.c_str(), strB.c_str()) == 0

# 还是上面的场景，直接用string类的compare函数
(gdb) b main.cpp:255 if strA.compare(strB) != 0

```

### 捕获exception {#1da18e}

gdb遇到未处理的exception时，并不会捕获处理。但是参考[Set Catchpoints](https://sourceware.org/gdb/onlinedocs/gdb/Set-Catchpoints.html)，可以使用`catch catch`命令来捕获exception。

