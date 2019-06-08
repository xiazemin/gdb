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



