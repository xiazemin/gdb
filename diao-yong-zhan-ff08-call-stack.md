1.查看调用栈信息:

a.**backtrace**:显示程序的调用栈信息，可以用**bt**缩写

b.**backtrace**_n_:显示程序的调用栈信息，只显示栈顶_n_桢\(frame\)

c.**backtrace -**_n_:显示程序的调用栈信息，只显示栈底部n桢\(frame\)

d.**set backtrace limit**_n_:设置**bt**显示的最大桢层数

e.**where**,**info stack**：都是**bt**的别名，功能一样

2.选择某一桢进行查看：

a.**frame**_n_:查看第_n_桢的信息

b.**frame**_addr_:查看pc地址为_addr_的桢的相关信息

c.**up**_n_:查看当前桢上面第_n_桢的信息

d.**down**_n_:查看当前桢下面第_n_桢的信息

3.frame信息内容：

a.用**backtrace**、**frame**_n_或者**frame**_addr_得到的简要信息内容：

（1）桢序号\(Frame number\)

（2）函数名

（3）Program counter（除非set print address off）（在程序当前执行到的那一桢，PC不会被显示）

（4）源代码文件名和行号

（5）函数的参数名和传入的值

b.用**info frame**、**info frame**_n_或者**info frame**_addr_得到的详细的信息内容：

（1）当前桢的地址

（2）下一桢的地址

（3）上一桢的地址

（4）源代码所用的程序的语言\(c/c++\)

（5）当前桢的参数的地址

（6）当前相中局部变量的地址

（7）PC（program counter）

（8）当前桢中存储的寄存器

4.info args：查看当前桢中的参数

5.info locals：查看当前桢中的局部变量

6.info catch：查看当前桢中的异常处理器（exception handlers）

