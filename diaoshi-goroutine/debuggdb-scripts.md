runtime-gdb.py 是用来查找.debug\_gdb\_scripts文件的

```
#
# Register all convenience functions and CLI commands
#
GoLenFunc()
GoCapFunc()
DTypeFunc()
GoroutinesCmd()
GoroutineCmd()
GoIfaceCmd()
```

```
import ( 
“fmt” 
“runtime” 
) 
func test(s string, x int) (r string) {
 r = fmt.Sprintf(“test: %s %d”, s, x)
  runtime.Breakpoint()
 return r
 }
```

通过runtime可以在代码中打断点

