\(gdb\) r e

Starting program:  e

No executable file specified.

Use the "file" or "exec-file" command.

\(gdb\) file e

Reading symbols from e...Reading symbols from /Users/didi/PhpstormProjects/c/c/gdb/e.dSYM/Contents/Resources/DWARF/e...done.

done.

对gdb签名后成功：

```
(gdb) r e
Starting program:  e
No executable file specified.
Use the "file" or "exec-file" command.
(gdb) file e
Reading symbols from e...Reading symbols from /Users/didi/PhpstormProjects/c/c/gdb/e.dSYM/Contents/Resources/DWARF/e...done.
done.
(gdb) b 10
Breakpoint 1 at 0x100000f26: file e.c, line 10.
(gdb) r
Starting program: /Users/didi/PhpstormProjects/c/c/gdb/e e
Unable to find Mach task port for process-id 5506: (os/kern) failure (0x5).
 (please check gdb is codesigned - see taskgated(8))
```

系统重启后可以用了。（注意一定要重启系统）

\(gdb\) b 10

Breakpoint 1 at 0x100000f26: file e.c, line 10.

\(gdb\) r

Starting program: /Users/didi/PhpstormProjects/c/c/gdb/e e



Breakpoint 1, main \(argc=2, argv=0x7fff5fbff0a8\) at e.c:11

11          j=0;



