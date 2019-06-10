**目前支持调试Go程序的GDB版本必须大于7.1**

**务必保证执行如下操作\(保证info goroutines可用\)**  
vim ~/.gdbinit 添加下面一行：  
add-auto-load-safe-path $GOROOT/src/pkg/runtime/runtime-gdb.py  
把$GOROOT替换为你自己的路径

**常用命令**  
list  
简写命令l,用来显示源代码,默认显示十行代码,后面可以带上参数显示的具体行，例如：list 15,显示十行代码,其中第15行在显示的十行里面的中间  
break  
简写命令b,用来设置断点,后面跟上参数设置断点的行数,例如b 10在第十行设置断点  
delete  
简写命令d,用来删除断点,后面跟上断点设置的序号,这个序号可以通过info breakpoints获取相应的设置的断点序号,如下是显示的设置断点序号  
backtrace  
简写命令bt,用来打印执行的代码过程  
info  
info命令用来显示信息,后面有几种参数,我们常用的有如下几种  
    info locals  
    显示当前执行的程序中的变量值  
    info breakpoints  
    显示当前设置的断点列表  
    info goroutines  
    显示当前执行的goroutine列表,带\*的表示当前执行的  
print  
简写命令p,用来打印变量或者其他信息,后面跟上需要打印的变量名,当然还有一些很有用的函数$len\(\)和$cap\(\),用来返回当前string,slices或者maps的长度和容量  
whatis  
用来显示当前变量的类型,后面跟上变量名  
next  
简写命令n,用来单步调试.跳到下一步.当有断点之后.可以输入n跳转到下一步继续执行  
coutinue  
简称命令c,用来跳出当前断点处,后面可以跟参数N,跳过多少次断点  
set variable  
该命令用来改变运行过程中的变量值，格式如：set variable &lt;var&gt;=&lt;value&gt;

[https://nebulas.gitbook.io/wiki/go-nebulas/develop/debuging-with-gdb](https://nebulas.gitbook.io/wiki/go-nebulas/develop/debuging-with-gdb)

