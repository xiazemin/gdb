```
(gdb) catch fork
Catchpoint 1 (fork)
(gdb) start
No symbol table loaded.  Use the "file" command.
(gdb) file mp
Reading symbols from mp...Reading symbols from /Users/didi/PhpstormProjects/c/c/gdb/mp.dSYM/Contents/Resources/DWARF/mp...done.
done.
(gdb) start
Temporary breakpoint 2 at 0x100000d26: file mp.c, line 30.
Starting program: /Users/didi/PhpstormProjects/c/c/gdb/mp 
warning: Error inserting catchpoint 1: Your system does not support this type
of catchpoint.

Temporary breakpoint 2, main () at mp.c:30
30          puts("main beign");
(gdb) l
25      
26      int main(void) {
27          int i;
28          pid_t rt;
29      
30          puts("main beign");
31      
32          for(i=0; i<_INT_PTX; ++i) {
33              // &i 是有问题的, 但是这里为了测试, 可以乱搞
---Type <return> to continue, or q <return> to quit---return
34              rt = fork();
(gdb) b 34
Breakpoint 3 at 0x100000d3f: file mp.c, line 34.
(gdb) info breakpoint
Num     Type           Disp Enb Address            What
1       catchpoint     keep n                      fork
3       breakpoint     keep y   0x0000000100000d3f in main at mp.c:34
(gdb) show follow-fork-mode
Debugger response to a program call of fork or vfork is "parent".
(gdb) show detach-on-fork
Whether gdb will detach the child of a fork is on.
(gdb) info inferiors
  Num  Description       Executable        
* 1    process 98195     /Users/didi/PhpstormProjects/c/c/gdb/mp 
(gdb) s
main beign
32          for(i=0; i<_INT_PTX; ++i) {
(gdb) info inferiors
  Num  Description       Executable        
* 1    process 98195     /Users/didi/PhpstormProjects/c/c/gdb/mp 
(gdb) s

Breakpoint 3, main () at mp.c:34
34              rt = fork();
(gdb) info inferiors
  Num  Description       Executable        
* 1    process 98195     /Users/didi/PhpstormProjects/c/c/gdb/mp 
(gdb) s
35              if(rt < 0) {
(gdb) info inferiors
  Num  Description       Executable        
* 1    process 98195     /Users/didi/PhpstormProjects/c/c/gdb/mp 
(gdb) set detach-on-fork of
(gdb) set detach-on-fork off
(gdb) showtach-on-fork 
Whether gdb will detach the child of a fork is off.
(gdb) s
39              if(rt == 0) {
(gdb) s
43          }
(gdb) s
32          for(i=0; i<_INT_PTX; ++i) {
(gdb) s

Breakpoint 3, main () at mp.c:34
34              rt = fork();
(gdb) s
35              if(rt < 0) {
(gdb) s
39              if(rt == 0) {
(gdb) info inferiors
  Num  Description       Executable        
* 1    process 98195     /Users/didi/PhpstormProjects/c/c/gdb/mp 
(gdb) c
Continuing.

Breakpoint 3, main () at mp.c:34
34              rt = fork();
(gdb) c
Continuing.
n=3, i=0
n=3, piyo = 10, _old=1
n=3, i=1
n=3, piyo = 10, _old=2
n=3, i=2
n=3, piyo = 10, _old=3
end
等待 有缘人[0]!
等待 有缘人[1]!
等待 有缘人[2]!
等待 有缘人[3]!




等待 有缘人[4]!
等待 有缘人[5]!
^C
Program received signal SIGINT, Interrupt.
0x00007fff98b242b2 in __semwait_signal () from /usr/lib/system/libsystem_kernel.dylib
(gdb) c
Continuing.
等待 有缘人[6]!
等待 有缘人[7]!
等待 有缘人[8]!
等待 有缘人[9]!
^C
Program received signal SIGINT, Interrupt.
0x00007fff98b242b2 in __semwait_signal () from /usr/lib/system/libsystem_kernel.dylib
(gdb) info inferiors
  Num  Description       Executable        
* 1    process 98195     /Users/didi/PhpstormProjects/c/c/gdb/mp 
(gdb) 

```



