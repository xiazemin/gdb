\(gdb\) help l

List specified function or line.

With no argument, lists ten more lines after or around previous listing.

"list -" lists the ten lines before a previous ten-line listing.

One argument specifies a line, and ten lines are listed around that line.

Two arguments with comma between specify starting and ending lines to list.

\(gdb\) l

No symbol table is loaded.  Use the "file" command.



在unix/linux系统下使用gdb进行调试时，如果出现：

No symbol table is loaded. Use the "file" command.

原因是没有在Makefile中添加-g调试参数，或者添加位置出错，解决的办法是在Makefile文件的第一行加上：

CFLAGS = -g

然后重新make即可。





