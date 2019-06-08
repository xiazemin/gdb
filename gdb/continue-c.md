\(gdb\) b 13

Breakpoint 3 at 0x100000f45: file e.c, line 13.

\(gdb\) c

Continuing.



Breakpoint 3, main \(argc=2, argv=0x7fff5fbff0a8\) at e.c:13

13              j+=5;

\(gdb\) c

Continuing.

now a=5



Breakpoint 3, main \(argc=2, argv=0x7fff5fbff0a8\) at e.c:13

13              j+=5;

\(gdb\) 



