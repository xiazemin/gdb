list命令显示多行源代码,从上次的位置开始显示,默认情况下,一次显示10行,第一次使用时,从代码起始位置显示

list n显示已第n行为中心的10行代码

l 8

list functionname显示以functionname的函数为中心的10行代码

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

$gcc -g fork\_signal.c -o fork\_signal

$gdb fork\_signal

GNU gdb \(GDB\) 7.12.1

\(gdb\) l runparent

186

187         int status;

188         wait\(&status\);

189     }

190

191     void runparent\(struct process \*pool\){

192         for\(int i=0;i&lt;pool\_size;i++\){

193             char str \[buffsize\];

194             sprintf\(str," send to child %d \n",pool\[i\].m\_pid\);

---Type &lt;return&gt; to continue, or q &lt;return&gt; to quit---return

195             int size = write\(pool\[i\].m\_pipefd\[0\], str, strlen\(str\)\);

