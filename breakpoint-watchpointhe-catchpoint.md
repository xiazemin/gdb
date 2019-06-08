1.Breakpoints相关命令：（Breakpoint的作用是让程序执行到某个特定的地方停止运行）

（1）设置breakpoint:

a.**break**_function_:在函数funtion入口处设置breakpoint

b.**break +**_offset_:在程序当前停止的行向前offset行处设置breakpoint

c.**break –**_offset_:在程序当前停止的行向衙offset行处设置breakpoint

d.**break**_linenum_:在当前源文件的第_linenum_行处设置breakpoint

e.**break**_filename_**:**_linenum_:在名为_filename_的源文件的第_linenum_行处设置breakpoint

f.**break**_filename_**:**_function_:在名为_filename_的源文件中的_function_函数入口处设置breakpoint

g.**break**\*_address_:在程序的地址_address_处设置breakpoint

h.**break**:

i.**break … if**_cond_:**…**代表上面讲到的任意一个可能的参数，在某处设置一个breakpoint,但且仅但_cond_为**true**时，程序停下来

j.**tbreak**_args_:设置一个只停止一次的breakpoints,_args_与**break**命令的一样。这样的breakpoint当第一次停下来后，就会被自己删除

k.**rbreak**_regex_:在所有符合正则表达式_regex_的函数处设置breakpoint

（2）**info breakpoints**\[_n_\]:查看第_n_个breakpoints的相关信息，如果省略了_n_，则显示所有breakpoints的相关信息

（3）pending breakpoints:是指设置在程序开始调试后加载的动态库中的位置处的breakpoints

a.**set breakpoint pending auto**: GDB缺省设置，询问用户是否要设置pending breakpoint

b.**set breakpoint pending on**: GDB当前不能识别的breakpoint自动成为pending breakpoint

c.**set breakpoint pending off**: GDB当前不能识别某个breakpoint时，直接报错

d.**show breakpoint pending**:查看GDB关于pending breakpoint的设置的行为\(auto, on, off\)

（4）breakpoints的删除：

a.**clear**:清除当前stack frame中下一条指令之后的所有breakpoints

b.**clear**_function_&**clear**_filename:function_:清除函数_function_入口处的breakpoints

c.**clear**_linenum_&**clear**_filename:linenum_:清除第_linenum_行处的breakpoints

d.**delete**\[**breakpoints**\] \[_range_…\]:删除由_range_指定的范围内的breakpoints，_range_范围是指breakpoint的序列号的范围

（5）breakpoints的禁用、启用:

a.**disable**\[**breakpoints**\] \[_range_…\]:禁用由_range_指定的范围内的breakpoints

b.**enable**\[**breakpoints**\] \[_range_…\]:启用由_range_指定的范围内的breakpoints

c.**enable**\[**breakpoints**\]**once**\[_range_…\]:只启用一次由_range_指定的范围内的breakpoints，等程序停下来后，自动设为禁用

d.**enable**\[**breakpoints**\]**delete**\[_range_…\]:启用_range_指定的范围内的breakpoints，等程序停下来后，这些breakpoints自动被删除

（6）条件breakpoints相关命令：

a.设置条件breakpoints可以通过**break**…**if**_cond_来设置，也可以通过**condition**_bnum expression_来设置，在这里首先要通过（1）中介绍的命令设置好breakpoints，然后用**condition**命令来指定某breakpoint的条件，该breakpoint由_bnum_指定，条件由_expression_指定

b.**condition**_bnum_:取消第_bnum_个breakpoint的条件

c.**ignore**_bnum count_:第_bnum_个breakpoint跳过_count_次后开始生效

（7）指定程序在某个breakpoint处停下来后执行一串命令：

a.格式：**commands**\[_bnum_\]

_… command-list …_

**end**

b.用途：指定程序在第_bnum_个breakpoint处停下来后，执行由_command-list_指定的命令串，如果没有指定_bnum_，则对最后一个breakpoint生效

c.取消命令列表：**commands**\[_bnum_\]

**end**

d.例子：

**break**foo if x&gt;0

**commands**

**silent**

**printf**"x is %d\n",x

**continue**

**end**

上面的例子含义：当x&gt;0时，在foo函数处停下来，然后打印出x的值，然后继续运行程序

2.Watchpoints相关命令：（Watchpoint的作用是让程序在某个表达式的值发生变化的时候停止运行，达到‘监视’该表达式的目的）

（1）设置watchpoints:

a.**watch**_expr_:设置写watchpoint，当应用程序写_expr_,修改其值时，程序停止运行

b.**rwatch**_expr_:设置读watchpoint，当应用程序读表达式_expr_时，程序停止运行

c.**awatch**_expr_:设置读写watchpoint,当应用程序读或者写表达式_expr_时，程序都会停止运行

（2）**info watchpoints**:查看当前调试的程序中设置的watchpoints相关信息

（3）watchpoints和breakpoints很相像，都有enable/disabe/delete等操作，使用方法也与breakpoints的类似

3.Catchpoints相关命令：（catchpoints的作用是让程序在发生某种事件的时候停止运行，比如C++中发生异常事件，加载动态库事件）

（1）设置catchpoints:

a.**catch**_event_:当事件_event_发生的时候，程序停止运行，这里event的取值有：

1）**throw**: C++抛出异常

2）**catch**: C++捕捉到异常

3）**exec**: exec被调用

4）**fork**: fork被调用

5）**vfork**: vfork被调用

6）**load**:加载动态库

7）**load**_libname_:加载名为_libname_的动态库

8）**unload**:卸载动态库

9）**unload**_libname_:卸载名为_libname_的动态库

10）**syscall**\[_args_\]:调用系统调用，_args_可以指定系统调用号，或者系统名称

b.**tcatch**_event_:设置只停一次的catchpoint，第一次生效后，该catchpoint被自动删除

（2）catchpoints和breakpoints很相像，都有enable/disabe/delete等操作，使用方法也与breakpoints的类似

