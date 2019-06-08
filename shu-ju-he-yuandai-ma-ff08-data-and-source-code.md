1.list命令（缩写为l）:

（1）**list**_linenum_:打印当前文件中第_linenum_行周围的源代码

（2）**list**_function_:打印_function_函数周围的源代码

（3）**list**:在上一次使用**list**命令的基础上，再多打印一些源代码

（4）**list -**:打印和上一次使用**list**命令一样的源代码

（5）**set listsize**_count_:设置**list**命令显示源代码的行数

（6）**show listsize**:查看**list**命令显示

2.用**edit**命令在GDB模式下编辑源代码：

（1）选择合适的编辑器，gdb会选择/bin/ex做为源代码编辑器，有些linux发行版上可能会没有安装/bin/ex，可以把编辑器修改为比较常见的vim，具体做法为：有启动gdb之前，在命令行执行**export**EDITOR=/usr/bin/vim（或者可以在.profile中设置EDITOR这个变量的值为/usr/bin/vim，这样就不用每次启动gdb的时候都去设置一下了）

（2）**edit**:编辑当前文件

（3）**edit**_number_:编辑当前文件的第_number_行

（4）**edit**_function_:编辑当前文件的_function_函数

（5）**edit**_filename_:_number_:编辑名为_filename_的文件的第_number_行

（6）**edit**_filename_:_function_:编辑名为_filename_的文件的_function_函数

3.设置源代码目录

（1）**directory**_dirname_:设置当前调试的程序的源代码目录为_dirname_

（2）**directory**:将当前调试的程序的源代码目录清空

（3）**show directories**:打印当前调试的程序的源代码目录

4.**print**命令打印数据：

（1）**print**_expr_:打印表达式_expr_的值

（2）**print /**_f expr_:以_f_指定的格式打印表达式_expr_的值

（3）**print**:打印上一次打印的表达式的值

（4）**print /**_f_:以_f_指定的形式打印上一次打印的表达式的值

（print支持的格式有：x-16进制整数，d-10进制整数，u-10进制无符号整数，o-8进制整数，t-2进制整数，a-地址形式整数，c-字符常量整数，f-浮点数）

5.GDB支持的表达式：

（1）Any kind of constant, variable or operator defined by the programming language you are using is valid in an expression inGDB.

（2）**@**_address_:把_address_指定的内存当作数组，语法为**p \*array@len**

（3）_file_**::**_variable_:指定变量_varialbe_被定义的文件_file_

（4）_function_**::**_varable_:指定变量_variable_被定义的函数_function_

（5）{_type_}_address_:把_address_指定的内存解释为_type_类型（类似于强制转型，更加强）

6.打印内存：**x**/_nfu addr_:

（1）_n_：重复次数，缺省是1

（2）_f_：打印格式，与print的相同的，还有s-C风格字符串，i-机器指令，缺省是x

（3）_u_：打印单位大小，b-byte，h-halfwords（2字节），w-words（4字节），g-Giant words（8字节）

7.自动打印：

（1）**display**/_f expr_\|_addr_：以_f_为格式，打印_expr_或者_addr_

（2）**undisplay**_dnums_，**delete display**dnums：删除第_dnums_个display点

（3）**disable display**_dnums_：禁用第_dnums_个display点

（4）**enable display**_dnums_：启用第_dnums_个display点

（5）**info display**：查看所有的display点

8.打印选项：

a.**set print**_field_:打开_field_选项

b.**set print**_field_**on**:打开_field_选项

c.**set print**_field_**off**:关闭_field_选项

d.**show print**_field_:查看_field_选项的打开、关闭情况

（1）**set print array**：以一种比较好看的方式打印数组，缺省是关闭的

（2）**set print elements**_num-of-elements_：设置GDB打印数据时显示元素的个数，缺省为200，设为0表示不限制\(unlimited\)

（3）**set print null-stop**：设置GDB打印字符数组的时候，遇到NULL时停止，缺省是关闭的

（4）**set print pretty**：设置GDB打印结构的时候，每行一个成员，并且有相应的缩进，缺省是关闭的

（5）**set print object**：设置GDB打印多态类型的时候，打印实际的类型，缺省为关闭

（6）**set print static-members**：设置GDB打印结构的时候，是否打印static成员，缺省是打开的

（7）**set print vtbl**：以漂亮的方式打印C++的虚函数表，缺省是关闭的

9.寄存器：

（1）**info registers**:查看当前桢中的各个寄存器的情况

（2）**info registers**_regname_:查看指定的寄存器

（3）各个寄存器：



10.内存copy:

（1）**dump**\[_format_\]**memory**_filename start\_addr end\_addr_

（2）**append**\[**binary**\]**memory**_filename start\_addr end\_addr_

（3）**restore**_filename_\[**binary**\]_bias start end_

11.在GDB中定义宏：

（1）**info macro**_macro_：查看宏_macro_的定义

（2）**macro define**_macro replacement-list_：（还没实现）

（3）**macro define**_macro\(arglist\) replacement-list_：（还没实现）

（4）**macro undef**_macro_：（还没实现）

12.修改程序的运行\(Altering Execution\)

（1）修改变量值：

a.**print**_v=value_:　修改变量值的同时，把修改后的值显示出来

b.**set**\[**var**\]_v=value_:　修改变量值，需要注意如果变量名与GDB中某个**set**命令中的关键字一样的话，前面加上**var**关键字

c.**whatis**_v_:查看变量类型

（2）**signal**_signal_:向程序发送信号_signal_,_signal_可以是信号的符号或数字形式，如果_signal_=0，那么程序将会继续运行，程序不会收到任何信号。

（3）**return**\[_expression_\]:中断函数执行，从当前位置直接返回。（注意：**finish**是把函数运行完，再返回，**return**是直接返回。）

（4）**call**_expr_:在GDB中调用应用程序中的函数

13.用户自定义命令：

（1）定义一个命令

**define**_commandname_

…

**end**

（2）条件语句：

**if**_cond-expr_

     …

**else**

     …

**end**

（4）循环语句：

**while**_cond-expr_

       …

**end**

（5）定义一个命令的文档信息，在**help**_commandname_的时候可以显示：

**document**_commandname_

        …

**end**

（6）$arg0…$arg9：表示命令行参数，最多10个

（7）**help user-defined**：查看所有的用户自定义命令

（8）**show user**commandname：查看自定义命令commandname的定义

（9）**help**commandname：查看自定义命令commandname的帮助信息

（10）**show max-user-call-depth**：查看用户自定义命令的最大递归调用深度

（11）**set max-user-call-depth**：设置用户自定义命令的最大递归调用深度

