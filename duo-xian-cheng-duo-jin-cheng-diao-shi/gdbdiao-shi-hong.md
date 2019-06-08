![](/assets/import.png)

首先看上面命令

**　　macro expand 宏\(参数\)**=&gt;**得到宏导出内容.**

**　　info macro 宏名 =&gt; 宏定义内容**

如果你需要用到上面gdb功能, 查看和导出宏的话.还需要gcc 支持,生成的时候加上**-ggdb3**如下

```
gcc -Wall -ggdb3 -o houge.
out
 houge.c
```

就可以使用了. 扩展一下 对于 gcc 编译的有个过程叫做 预编译** gcc -E -o \*.i \*.c.**

这时候处理多数宏,直接展开, 也可以查看最后结果. 也算也是一个黑科技.

