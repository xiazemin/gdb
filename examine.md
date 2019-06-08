**x /23dw a** 意思是  查看 从a地址开始 23个 4字节 有符号十进制数 输出.

关于x 更加详细见下面

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

```
用gdb查看内存格式: 
    x 
/
nfu ptr

说明
x 是 examine 的缩写
n表示要显示的内存单元的个数

f表示显示方式, 可取如下值
x 按十六进制格式显示变量。
d 按十进制格式显示变量。
u 按十进制格式显示无符号整型。
o 按八进制格式显示变量。
t 按二进制格式显示变量。
a 按十六进制格式显示变量。
i 指令地址格式
c 按字符格式显示变量。
f 按浮点数格式显示变量。

u表示一个地址单元的长度
b表示单字节，
h表示双字节，
w表示四字节，
g表示八字节

Format letters are o(octal), x(hex), d(
decimal
), u(unsigned 
decimal
),
t(binary), f(
float
), a(address), i(instruction), c(
char
) and s(
string
).
Size letters are b(
byte
), h(halfword), w(word), g(giant, 
8
 bytes)

ptr 表示从那个地址开始
```

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

这个命令常用于监测内存变化.调试中特别常用.

