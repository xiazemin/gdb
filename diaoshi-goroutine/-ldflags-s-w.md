（目前IDE支持调试Go程序，用的也是GDB。要求GDB 7.1以上）

以下内容来自雨痕的《Go语言学习笔记》（[下载Go资源](http://bbs.studygolang.com/forum.php?mod=viewthread&tid=10&extra=page%3D1)）：

默认情况下，编译过的二进制文件已经包含了 DWARFv3 调试信息，只要 GDB7.1 以上版本都可以进行调试。 在OSX下，如无法执行调试指令，可尝试用sudo方式执行gdb。

删除调试符号：go build -ldflags “-s -w”

* -s: 去掉符号信息。
* -w: 去掉DWARF调试信息。

关闭内联优化：go build -gcflags “-N -l”

