通过log库输出日志，我们可以对程序进行异常分析和问题追踪。但有时候，我也希望能有更直接的程序跟踪及定位工具能够帮助我们更方便快捷的追踪、定位问题，最直观的感觉还是使用调试器。Linux平台下，原生的C/C++程序，我们往往使用gdb进行程序调试，切换到Golang，我们同样还是可以使用gdb进行调试。同时我们还可以使用golang实现的调试器dlv进行调试。以下内容是我对gdb以及dlv使用及对比总结



## 准备工作 {#准备工作}

为展示整个调试过程，准备了一个演示项目**GoDbg**,整个目录结构如下所示

```
[lday@alex GoDbg]$ tree
.
├── main.go
└── mylib
    └── dbgTest.go

```

其中，main.go为主函数入口，而dbgTest.go启动多个goroutine，用于演示调试操作。  
main.go:  


| 1234567891011121314151617181920212223242526272829 | package mainimport \("GoWorks/GoDbg/mylib""fmt""os"\)funcmain\(\) {	fmt.Println\("Golang dbg test..."\)var argc = len\(os.Args\)var argv = append\(\[\]string{}, os.Args...\)	fmt.Printf\("argc:%d\n", argc\)	fmt.Printf\("argv:%v\n", argv\)var var1 = 1var var2 = "golang dbg test"var var3 = \[\]int{1, 2, 3}var var4 mylib.MyStruct	var4.A = 1	var4.B = "golang dbg my struct field B"	var4.C = map\[int\]string{1: "value1", 2: "value2", 3: "value3"}	var4.D = \[\]string{"D1", "D2", "D3"}	mylib.DBGTestRun\(var1, var2, var3, var4\)	fmt.Println\("Golang dbg test over"\)} |
| :--- | :--- |




dbgTest.go:  


| 1234567891011121314151617181920212223242526272829303132333435363738394041424344454647484950515253545556575859606162636465 | package mylibimport \("fmt""sync""time"\)type MyStruct struct {	A int	B string	C map\[int\]string	D \[\]string}funcDBGTestRun\(var1 int, var2 string, var3 \[\]int, var4 MyStruct\) {	fmt.Println\("DBGTestRun Begin!\n"\)	waiter := &sync.WaitGroup{}	waiter.Add\(1\)go RunFunc1\(var1, waiter\)	waiter.Add\(1\)go RunFunc2\(var2, waiter\)	waiter.Add\(1\)go RunFunc3\(&var3, waiter\)	waiter.Add\(1\)go RunFunc4\(&var4, waiter\)	waiter.Wait\(\)	fmt.Println\("DBGTestRun Finished!\n"\)}funcRunFunc1\(variable int, waiter \*sync.WaitGroup\) {	fmt.Printf\("var1:%v\n", variable\)for {if variable != 123456 {continue		} else {break		}	}	time.Sleep\(10 \* time.Second\)	waiter.Done\(\)}funcRunFunc2\(variable string, waiter \*sync.WaitGroup\) {	fmt.Printf\("var2:%v\n", variable\)	time.Sleep\(10 \* time.Second\)	waiter.Done\(\)}funcRunFunc3\(pVariable \*\[\]int, waiter \*sync.WaitGroup\) {	fmt.Printf\("\*pVar3:%v\n", \*pVariable\)	time.Sleep\(10 \* time.Second\)	waiter.Done\(\)}funcRunFunc4\(pVariable \*MyStruct, waiter \*sync.WaitGroup\) {	fmt.Printf\("\*pVar4:%v\n", \*pVariable\)	time.Sleep\(10 \* time.Second\)	waiter.Done\(\)} |
| :--- | :--- |




在对程序进行调试前，我们需要对目标程序进行调试版本程序的编译。C/C++程序，我们会通过gcc/g++进行编译、链接时加入`-g3`等参数，使得程序编译时带入调试信息，进而让调试器能够最终并解释相关的程序代码。同样的，在我们对Golang程序进行调试时，我们也需要加入相应的编译、链接选项：`-gcflags="-N -l"`，生成程序调试信息（`-N -l`用于关闭编译器的内联优化）。编译GoDbg项目指令：`go build -gcflags="-N -l" GoWorks/GoDbg`

## gdb调试程序 {#gdb调试程序}

因为gdb对Golang的支持也是在不断完善中，为使用gdb调试Golang程序，建议将gdb升级到相对较新版本，目前，我使用的版本是**gdb7.10**。  
大多数命令在使用gdb调试C/C++时都会用到，详细说明可参考：[Debugging Go Code with GDB](https://golang.org/doc/gdb)，具体操作如下：

1. 启动调试程序（`gdb`）

   ```
   [lday@alex GoDbg]$ gdb ./GoDbg

   ```

2. 在main函数上设置断点（`b`）

   ```
   (gdb) b main.main
   Breakpoint 1 at 0x401000: file /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go, line 9.

   ```

3. 带参数启动程序（`r`）

   ```
   (gdb) r arg1 arg2
   Starting program: /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/GoDbg arg1 arg2
   [New LWP 8412]
   [New LWP 8413]
   [New LWP 8414]
   [New LWP 8415]

   Breakpoint 1, main.main () at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:9
   9    func main() {

   ```

4. 在文件dbgTest.go上通过行号设置断点（`b`）

   ```
   (gdb) b dbgTest.go:16
   Breakpoint 3 at 0x457960: file /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go, line 16.

   ```

5. 查看断点设置情况（`info b`）

   ```
   (gdb) info b
   Num     Type           Disp Enb Address            What
   1       breakpoint     keep y   0x0000000000401000 in main.main 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:9
       breakpoint already hit 1 time
   2       breakpoint     keep y   0x0000000000401000 in main.main 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:9
       breakpoint already hit 1 time
   3       breakpoint     keep y   0x0000000000457960 in GoWorks/GoDbg/mylib.DBGTestRun 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:16

   ```

6. 禁用断点（`dis n`）

   ```
   (gdb) dis 1   
   (gdb) info b
   Num     Type           Disp Enb Address            What
   1       breakpoint     keep n   0x0000000000401000 in main.main 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:9
       breakpoint already hit 1 time
   2       breakpoint     keep y   0x0000000000401000 in main.main 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:9
       breakpoint already hit 1 time
   3       breakpoint     keep y   0x0000000000457960 in GoWorks/GoDbg/mylib.DBGTestRun 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:16

   ```

7. 删除断点（`del n`）

   ```
   (gdb) del 1
   (gdb) info b
   Num     Type           Disp Enb Address            What
   2       breakpoint     keep y   0x0000000000401000 in main.main 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:9
       breakpoint already hit 1 time
   3       breakpoint     keep y   0x0000000000457960 in GoWorks/GoDbg/mylib.DBGTestRun 
                                                      at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:16

   ```

8. 断点后继续执行（`c`）

   ```
   (gdb) c
   Continuing.
   Golang dbg test...
   argc:3
   argv:[/home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/GoDbg arg1 arg2]

   Breakpoint 3, GoWorks/GoDbg/mylib.DBGTestRun (var1=1, var2="golang dbg test")
       at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:16
   16    func DBGTestRun(var1 int, var2 string, var3 []int, var4 MyStruct) {
   (gdb) 

   ```

9. 显示代码（`l`）

   ```
   (gdb) l
   11        B string
   12        C map[int]string
   13        D []string
   14    }
   15    
   16    func DBGTestRun(var1 int, var2 string, var3 []int, var4 MyStruct) {
   17        fmt.Println("DBGTestRun Begin!\n")
   18        waiter := 
   &
   sync.WaitGroup{}
   19    
   20        waiter.Add(1)

   ```

10. 单步执行（`n`）

    ```
    (gdb) n
    DBGTestRun Begin!

    18        waiter := 
    &
    sync.WaitGroup{}

    ```

11. 打印变量信息（`print/p`）  
    在进入DBGTestRun的地方设置断点\(`b dbgTest.go:16`\)，进入该函数后，通过p命令显示对应变量：

    ```
    (gdb) l 17
    12        C map[int]string
    13        D []string
    14    }
    15    
    16    func DBGTestRun(var1 int, var2 string, var3 []int, var4 MyStruct) {
    17        fmt.Println("DBGTestRun Begin!\n")
    18        waiter := 
    &
    sync.WaitGroup{}
    19    
    20        waiter.Add(1)
    21        go RunFunc1(var1, waiter)
    (gdb) p var1 
    $3 = 1
    (gdb) p var2
    $4 = "golang dbg test"
    (gdb) p var3
    No symbol "var3" in current context.

    ```

    _**从上面的输出我们可以看到一个很奇怪的事情，虽然DBGTestRun有4个参数传入，但是，似乎var3和var4 gdb无法识别，在后续对dlv的实验操作中，我们发现，dlv能够识别var3， var4.**_

12. 查看调用栈（`bt`），切换调用栈（`f n`），显示当前栈变量信息

    ```
    (gdb) bt
    #0  GoWorks/GoDbg/mylib.DBGTestRun (var1=1, var2="golang dbg test")
        at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:17
    #1  0x00000000004018c2 in main.main () at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:27
    (gdb) f 1
    #1  0x00000000004018c2 in main.main () at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/main.go:27
    27        mylib.DBGTestRun(var1, var2, var3, var4)
    (gdb) l
    22        var4.A = 1
    23        var4.B = "golang dbg my struct field B"
    24        var4.C = map[int]string{1: "value1", 2: "value2", 3: "value3"}
    25        var4.D = []string{"D1", "D2", "D3"}
    26    
    27        mylib.DBGTestRun(var1, var2, var3, var4)
    28        fmt.Println("Golang dbg test over")
    29    }
    (gdb) print var1 
    $5 = 1
    (gdb) print var2
    $6 = "golang dbg test"
    (gdb) print var3
    $7 =  []int = {1, 2, 3}

    (gdb) print var4
    $8 = {A = 1, B = "golang dbg my struct field B", C = map[int]string = {[1] = "value1", [2] = "value2", [3] = "value3"}, 
    D =  []string = {"D1", "D2", "D3"}}

    ```

13. 显示goroutine列表（`info goroutines`）  
    当程序执行到`dbgTest.go:23`时，程序通过go启动了第一个goroutine，并执行`RunFunc1()`，我们可以通过上述命令查看goroutine列表

    ```
    (gdb) n
    23        waiter.Add(1)
    (gdb) info goroutines
    * 1 running  runtime.systemstack_switch
      2 waiting  runtime.gopark
      17 waiting  runtime.gopark
      18 waiting  runtime.gopark
      19 runnable GoWorks/GoDbg/mylib.RunFunc1

    ```

14. 查看goroutine的具体情况（`goroutine n cmd`）

    ```
    (gdb) goroutine 19 bt
    #0  GoWorks/GoDbg/mylib.RunFunc1 (variable=1, waiter=0xc8200721f0)
        at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:36
    #1  0x0000000000456df1 in runtime.goexit () at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/asm_amd64.s:1998
    #2  0x0000000000000001 in ?? ()
    #3  0x000000c8200721f0 in ?? ()
    #4  0x0000000000000000 in ?? ()

    ```

    我们可以通过上述指令查看goroutine 9的调用栈，显然，该goroutine正在执行`dbgTest.go:36`行的函数：`RunFunc1`的goroutine。我们通过`goroutine 19 info args`等命令来查看该goroutine最顶层调用栈的变量信息，但是，如果我们需要查看的信息不再最顶层调用栈上，则很遗憾，gdb没法输出

    ```
    (gdb) goroutine 19 info args
    variable = 1
    waiter = 0xc8200721f0
    (gdb) goroutine 19 p waiter 
    $1 = (struct sync.WaitGroup *) 0xc8200721f0

    (gdb) goroutine 19 p *waiter 
    $2 = {state1 = "\000\000\000\000\001\000\000\000\000\000\000", sema = 0}

    ```

    当我们执行到第26行，第2个goroutine被我们启动时，再次查看goroutine列表：

    ```
    (gdb) n
    26        waiter.Add(1)
    (gdb) info goroutines
    * 1 running  runtime.systemstack_switch
      2 waiting  runtime.gopark
      17 waiting  runtime.gopark
      18 waiting  runtime.gopark
    * 19 running  syscall.Syscall
      20 runnable GoWorks/GoDbg/mylib.RunFunc2

    ```

    此时我们再次查看goroutine 19的状态

    ```
    (gdb) goroutine 19 bt
    #0  syscall.Syscall () at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/syscall/asm_linux_amd64.s:19
    #1  0x00000000004ab95f in syscall.write (fd=1, p= []uint8 = {...}, n=859530587568, err=...)
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/syscall/zsyscall_linux_amd64.go:1064
    #2  0x00000000004ab40d in syscall.Write (fd=5131648, p= []uint8, n=0, err=...)
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/syscall/syscall_unix.go:180
    #3  0x000000000046c928 in os.(*File).write (f=0xc820084008, b= []uint8, n=4571929, err=...)
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/os/file_unix.go:255
    #4  0x000000000046aa24 in os.(*File).Write (f=0xc82008a000, b= []uint8 = {...}, n=7, err=...)
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/os/file.go:144
    #5  0x000000000045c707 in fmt.Fprintf (w=..., format="var1:%v\n", a= []interface {} = {...}, n=7, err=...)
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:190
    #6  0x000000000045c7b4 in fmt.Printf (format="var1:%v\n", a= []interface {} = {...}, n=7, err=...)
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:197
    #7  0x00000000004583eb in GoWorks/GoDbg/mylib.RunFunc1 (variable=1, waiter=0xc8200721f0)
        at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:37
    #8  0x0000000000456df1 in runtime.goexit () at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/asm_amd64.s:1998
    #9  0x0000000000000001 in ?? ()
    #10 0x000000c8200721f0 in ?? ()
    #11 0x0000000000000000 in ?? ()

    ```

    从第7，8层调用栈我们可以看到，此时goroutine 19已经进入到`RunFunc1`的`fmt.Printf`函数中，当我们尝试在goroutine 19上切换栈时，gdb报错：

    ```
    (gdb) goroutine 19 f 7
    #7  0x00000000004583eb in GoWorks/GoDbg/mylib.RunFunc1 (variable=1, waiter=0xc8200721f0)
        at /home/lday/Works/Go_Works/GoLocalWorks/src/GoWorks/GoDbg/mylib/dbgTest.go:37
    37        fmt.Printf("var1:%v\n", variable)
    Python Exception 
    <
    class 'gdb.error'
    >
     Frame is invalid.: 
    Error occurred in Python command: Frame is invalid.

    ```

    似乎gdb不允许我们在goroutine上做调用栈的切换，因此我们没法在这种状态下查看某层调用栈的变量信息。_**缺少在goroutine上不同frame的变量查看，个人感觉gdb调试Golang程序功能大打折扣，在后面对dlv的实验操作中我们可以看到，dlv可以！**_

## dlv调试程序 {#dlv调试程序}

尝试了”老牌”调试器gdb，我们再来试试新进的Golang原生调试器[delve\(dlv\)](https://github.com/derekparker/delve)。

> Delve is a debugger for the Go programming language. The goal of the project is to provide a simple, full featured debugging tool for Go

dlv是Golang实现的Golang调试器，目前dlv对windows平台的支持似乎不是很好，我在windows平台调试，dlv无法找到目标程序的源代码，因此建议在Linux平台下调试Golang程序时使用。使用dlv前，需在本地通过`go get github.com/derekparker/delve/cmd/dlv`进行安装。dlv的详细介绍可参见github上的[delve介绍](https://github.com/derekparker/delve)。以下是具体操作说明

1. 带参数启动程序（`dlv exec ./GoDbg -- arg1 arg2`）

   ```
   [lday@alex GoDbg]$ dlv exec ./GoDbg -- arg1 arg2 
   Type 'help' for list of commands.
   (dlv) 

   ```

2. 在main函数上设置断点（`b`）

   ```
   (dlv) b main.main
   Breakpoint 1 set at 0x40101b for main.main() ./main.go:9

   ```

3. 启动调试，断点后继续执行（`c`）

   ```
   (dlv) c

   >
    main.main() ./main.go:9 (hits goroutine(1):1 total:1) (PC: 0x40101b)
        4:        "GoWorks/GoDbg/mylib"
        5:        "fmt"
        6:        "os"
        7:    )
        8:    
   =
   >
      9:    func main() {
       10:        fmt.Println("Golang dbg test...")
       11:    
       12:        var argc = len(os.Args)
       13:        var argv = append([]string{}, os.Args...)
       14:    

   ```

4. 在文件dbgTest.go上通过行号设置断点（`b`）

   ```
   (dlv) b dbgTest.go:17
   Breakpoint 2 set at 0x457f51 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:17
   (dlv) b dbgTest.go:23
   Breakpoint 3 set at 0x4580d0 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:23
   (dlv) b dbgTest.go:26
   Breakpoint 4 set at 0x458123 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:26
   (dlv) b dbgTest.go:29
   Breakpoint 5 set at 0x458166 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:29

   ```

5. 显示所有断点列表（`bp`）

   ```
   (dlv) bp
   Breakpoint unrecovered-panic at 0x429690 for runtime.startpanic() /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/panic.go:524 (0)
   Breakpoint 1 at 0x40101b for main.main() ./main.go:9 (1)
   Breakpoint 2 at 0x457f51 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:17 (0)
   Breakpoint 3 at 0x4580d0 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:23 (0)
   Breakpoint 4 at 0x458123 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:26 (0)
   Breakpoint 5 at 0x458166 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:29 (0)

   ```

   dlv似乎没有提供类似gdb`dis x`，禁止某个断点的功能，在文档中暂时没有查到。不过这个功能用处不大。

6. 删除某个断点（`clear x`）

   ```
   (dlv) clear 5
   Breakpoint 5 cleared at 0x458166 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:29
   (dlv) bp
   Breakpoint unrecovered-panic at 0x429690 for runtime.startpanic() /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/panic.go:524 (0)
   Breakpoint 1 at 0x40101b for main.main() ./main.go:9 (1)
   Breakpoint 2 at 0x457f51 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:17 (0)
   Breakpoint 3 at 0x4580d0 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:23 (0)
   Breakpoint 4 at 0x458123 for GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:26 (0)

   ```

7. 显示当前运行的代码位置（`ls`）

   ```
   (dlv) ls

   >
    GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:17 (hits goroutine(1):1 total:1) (PC: 0x457f51)
       12:        C map[int]string
       13:        D []string
       14:    }
       15:    
       16:    func DBGTestRun(var1 int, var2 string, var3 []int, var4 MyStruct) {
   =
   >
     17:        fmt.Println("DBGTestRun Begin!\n")
       18:        waiter := 
   &
   sync.WaitGroup{}
       19:    
       20:        waiter.Add(1)
       21:        go RunFunc1(var1, waiter)
       22:    

   ```

8. 查看当前调用栈信息（`bt`）

   ```
   (dlv) bt
   0  0x0000000000457f51 in GoWorks/GoDbg/mylib.DBGTestRun
      at ./mylib/dbgTest.go:17
   1  0x0000000000401818 in main.main
      at ./main.go:27
   2  0x000000000042aefb in runtime.main
      at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/proc.go:188
   3  0x0000000000456df0 in runtime.goexit
      at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/asm_amd64.s:1998

   ```

9. 输出变量信息（`print/p`）

   ```
   (dlv) print var1
   1
   (dlv) print var2
   "golang dbg test"
   (dlv) print var3
   []int len: 3, cap: 3, [1,2,3]
   (dlv) print var4
   GoWorks/GoDbg/mylib.MyStruct {
       A: 1,
       B: "golang dbg my struct field B",
       C: map[int]string [
           1: "value1", 
           2: "value2", 
           3: "value3", 
       ],
       D: []string len: 3, cap: 3, ["D1","D2","D3"],}

   ```

   _**类比gdb调试，我们看到，之前我们使用gdb进行调试时，发现gdb在此时无法输出var3, var4的内容，而dlv可以**_

10. 在第n层调用栈上执行相应指令（`frame n cmd`）

    ```
    (dlv) frame 1 ls
        22:        var4.A = 1
        23:        var4.B = "golang dbg my struct field B"
        24:        var4.C = map[int]string{1: "value1", 2: "value2", 3: "value3"}
        25:        var4.D = []string{"D1", "D2", "D3"}
        26:    
    =
    >
      27:        mylib.DBGTestRun(var1, var2, var3, var4)
        28:        fmt.Println("Golang dbg test over")
        29:    }

    ```

    `frame 1 ls`将显示程序在第1层调用栈上的具体实行位置

11. 查看goroutine的信息（`goroutines`）  
    当我们执行到`dbgTest.go:26`时，我们已经启动了两个goroutine

    ```
    (dlv) 

    >
     GoWorks/GoDbg/mylib.DBGTestRun() ./mylib/dbgTest.go:26 (hits goroutine(1):1 total:1) (PC: 0x458123)
        21:        go RunFunc1(var1, waiter)
        22:    
        23:        waiter.Add(1)
        24:        go RunFunc2(var2, waiter)
        25:    
    =
    >
      26:        waiter.Add(1)
        27:        go RunFunc3(
    &
    var3, waiter)
        28:    
        29:        waiter.Add(1)
        30:        go RunFunc4(
    &
    var4, waiter)
        31:    

    ```

    此时我们来查看程序的goroutine状态信息

    ```
    (dlv) goroutines
    [6 goroutines]
    * Goroutine 1 - User: ./mylib/dbgTest.go:26 GoWorks/GoDbg/mylib.DBGTestRun (0x458123) (thread 9022)
      Goroutine 2 - User: /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/proc.go:263 runtime.gopark (0x42b2d3)
      Goroutine 3 - User: /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/proc.go:263 runtime.gopark (0x42b2d3)
      Goroutine 4 - User: /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/proc.go:263 runtime.gopark (0x42b2d3)
      Goroutine 5 - User: ./mylib/dbgTest.go:39 GoWorks/GoDbg/mylib.RunFunc1 (0x4583eb) (thread 9035)
      Goroutine 6 - User: /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/format.go:130 fmt.(*fmt).padString (0x459545)

    ```

    从输出的信息来看，先启动的goroutine 5，执行`RunFunc1`，此时还没有执行`fmt.Printf`，而后启动的goroutine 6，执行`RunFunc2`，则已经进入到`fmt.Printf`的内部调用过程中了

12. 进一步查看goroutine信息（`goroutine x`）  
    接第11步的操作，此时我想查看goroutine 6的具体执行情况，则执行`goroutine 6`

    ```
    (dlv) goroutine 6
    Switched from 1 to 6 (thread 9022)

    ```

    在此基础上，执行`bt`，则可以看到当前goroutine的调用栈情况

    ```
    (dlv) bt
     0  0x0000000000454730 in runtime.systemstack_switch
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/asm_amd64.s:245
     1  0x000000000040f700 in runtime.mallocgc
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/malloc.go:643
     2  0x000000000040fc43 in runtime.rawmem
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/malloc.go:809
     3  0x000000000043c2a5 in runtime.growslice
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/slice.go:95
     4  0x000000000043c015 in runtime.growslice_n
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/slice.go:44
     5  0x0000000000459545 in fmt.(*fmt).padString
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/format.go:130
     6  0x000000000045a13f in fmt.(*fmt).fmt_s
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/format.go:322
     7  0x000000000045e905 in fmt.(*pp).fmtString
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:518
     8  0x000000000046200f in fmt.(*pp).printArg
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:797
     9  0x0000000000468a8d in fmt.(*pp).doPrintf
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:1238
    10  0x000000000045c654 in fmt.Fprintf
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:188

    ```

    此时输出了10层调用栈，但似乎最原始的我自身程序dbgTest.go的调用栈没有输出， 可以通过`bt`加depth参数，设定bt的输出深度，进而找到我们自己的调用栈，例如`bt 13`

    ```
    (dlv) bt 13
    ...
    10  0x000000000045c654 in fmt.Fprintf
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:188
    11  0x000000000045c74b in fmt.Printf
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/print.go:197
    12  0x000000000045846f in GoWorks/GoDbg/mylib.RunFunc2
        at ./mylib/dbgTest.go:50
    13  0x0000000000456df0 in runtime.goexit
        at /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/asm_amd64.s:1998

    ```

    我们看到，我们自己dbgTest.go的调用栈在第12层。当前goroutine已经不再我们自己的调用栈上，而是进入到系统函数的调用中，在这种情况下，使用gdb进行调试时，我们发现，此时我们没有很好的方法能够输出我们需要的调用栈变量信息。**dlv可以!**此时只需简单的通过`frame x cmd`就可以输出我们想要的调用栈信息了

    ```
    (dlv) frame 12 ls
        45:        time.Sleep(10 * time.Second)
        46:        waiter.Done()
        47:    }
        48:    
        49:    func RunFunc2(variable string, waiter *sync.WaitGroup) {
    =
    >
      50:        fmt.Printf("var2:%v\n", variable)
        51:        time.Sleep(10 * time.Second)
        52:        waiter.Done()
        53:    }
        54:    
        55:    func RunFunc3(pVariable *[]int, waiter *sync.WaitGroup) {
    (dlv) frame 12 print variable 
    "golang dbg test"
    (dlv) frame 12 print waiter
    *sync.WaitGroup {
        state1: [12]uint8 [0,0,0,0,2,0,0,0,0,0,0,0],
        sema: 0,}

    ```

    多好的功能啊！

13. 查看当前是在哪个goroutine上（`goroutine`）  
    当使用`goroutine`不带参数时，dlv就会显示当前goroutine信息，这可以帮助我们在调试时确认是否需要做goroutine切换

    ```
    (dlv) goroutine
    Thread 9022 at ./mylib/dbgTest.go:26
    Goroutine 6:
        Runtime: /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/runtime/asm_amd64.s:245 runtime.systemstack_switch (0x454730)
        User: /home/lday/Tools/Dev_Tools/Go_Tools/go_1_6_2/src/fmt/format.go:130 fmt.(*fmt).padString (0x459545)
        Go: ./mylib/dbgTest.go:26 GoWorks/GoDbg/mylib.DBGTestRun (0x458123)

    ```

### dlv前端\(gdlv\) {#dlv前端-gdlv}

dlv提供了类似gdb的cli调试系统，而有第三方还提供了dlv的GUI前端\([gdlv](https://github.com/aarzilli/gdlv)\)，对于那些习惯了使用GUI进行调试的人来说，结合gdlv和dlv，调试会更加方便。gdlv有个问题是：他无法在xwindows server上运行，只能在server本地运行。  
[![](https://raw.githubusercontent.com/aarzilli/gdlv/master/doc/screen.png)](https://raw.githubusercontent.com/aarzilli/gdlv/master/doc/screen.png)

## 结论 {#结论}

综合比较两个Golang程序调试器gdb和dlv，我认为dlv的功能更为完善，更能满足实际调试时的功能需求。两者的优缺点比较大致如下

| 调试器 | 优势 | 不足 |
| :--- | :--- | :--- |
| dlv | 对goroutine环境调试支持比较完善 | 暂时没找到 |
| gdb | 符合现有的调试习惯，类似C/C++调试指令都有 | 对goroutine场景支持不足，不能很好的应对goroutine的调试 |

  


