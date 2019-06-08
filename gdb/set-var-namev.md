\(gdb\) set var kReadEvent=1

Cannot access memory at address 0x100001d78

\(gdb\) set var port=8088

Cannot access memory at address 0x100002108

set var name=value

在程序运行中动态改变变量的值

```
(gdb) print j
$1 = 5
(gdb) set var j=-1
(gdb) print j
$2 = -1

```



