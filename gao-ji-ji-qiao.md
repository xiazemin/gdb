### 加载独立的调试信息 {#c1ed35}

gdb调试的时候可以从单独的符号文件中加载调试信息。

```
(gdb) exec-file test
(gdb) symbol-file test.debug

```

test是移除了调试信息的可执行文件, test.debug是被移除后单独存储的调试信息。参考[stackoverflow上的一个问题](http://stackoverflow.com/questions/866721/how-to-generate-gcc-debug-symbol-outside-the-build-target)，可以如下分离调试信息:

```
# 编译程序，带调试信息(-g)
gcc -g -o test main.c

# 拷贝调试信息到test.debug
objcopy --only-keep-debug test test.debug

# 移除test中的调试信息
strip --strip-debug --strip-unneeded test

# 然后启动gdb
gdb -s test.debug -e test

# 或这样启动gdb
gdb
(gdb) exec-file test
(gdb) symbol-file test.debug

```

分离出的调试信息test.debug还可以链接回可执行文件test中

```
objcopy --add-gnu-debuglink test.debug test

```

然后就可以正常用addr2line等需要读取调试信息的程序了

```
addr2line -e test 0x401c23

```

更多内容可阅读[GDB: Debugging Information in Separate Files](https://sourceware.org/gdb/onlinedocs/gdb/Separate-Debug-Files.html)。

  


