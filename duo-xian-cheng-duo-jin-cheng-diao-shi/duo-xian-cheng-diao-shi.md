# [喜欢兰花山丘](https://www.cnblogs.com/life2refuel/)

目送飞云, 一切安好

## [Linux基础 30分钟GDB调试快速突破](https://www.cnblogs.com/life2refuel/p/5396538.html)

**引言 Linus心灵鸡汤**

　　在\*nix开发中有道卡叫gdb调试,不管你怎么搞. 它依然在那丝毫不会松动.今天致敬一个**活着的传奇 Linus Torvalds**

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160415185209520-596388508.jpg)

　　Unix 始于上个世纪60年代，在70年代得到了迅猛的发展，

这时候的李纳斯还躺在祖父公寓的摇篮里睡大觉，如果不是后来 Unix 王国自乱阵脚，

出现阵营分裂和法律纠纷，可能 Linux 系统根本都不会出现。真实的情况是，

Unix 浪费了大把的时间和机会，似乎就是为了等待这个大鼻子、头发纷乱的芬兰小子长大，然后一决高下。

　　李纳斯赢得了自己的时间，他一刻不停的磨练自己的技艺，在清晨的微光中练习算法，

在赫尔辛基的雪山上编译代码，随时随地补充的粮草和武器。

二十一年之后，李纳斯抚着雪亮的刀锋上路了，他要去追寻属于程序员的最高荣耀。\[

**　　I simply know better than you, that's why I'm your god.**

**    　　　　　　　　　　　　　　　　　　　 　　- -  Linus Torvalds**

\]



**前言  gdb 开始调试开始上手**

**1. 开启core, 采集程序崩溃的状态**

　　首先你跟着我做开启core崩溃状态采集. 可以通过 ulimit -c 查看 如果是0表示没有开启. 开启按照下面操作

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

```
su root

vi 
/etc/
profile
Shift 
+
 G
i
# No core files by 
default
0
, unlimited 
is
 oo
ulimit 
-S -c unlimited 
>
 /dev/
null
2
>
&
1

wq
!


source 
/etc/profile
```

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

上面shell 操作是 在 /etc/profile 最后一行添加 上面设置全局开启 core文件调试,大小不限. 最后 立即生效.

再跟着我做, 因为生成的core文件同名会覆盖. 这里为其加上一个 core命名规则, 让其变成 \[core.pid\] 格式.

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

```
su root

vi 
/etc/
sysctl.conf
Shift 
+
 G
i

# open, add core.pid 

kernel.core_pattern = ./core_%t_%p_%e


kernel.core_uses_pid = 1


wq
!


sysctl 
-p /etc/sysctl.conf
```

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

在 /etc/sysctl.conf 文件中添加系统配置. 后面立即启用. 最后是下面状态表示core启用都搞好了.

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416131202723-587345601.png)

\(上面是ubuntu 15.10 环境中, 后面测试用的是centos 6.4\)



**2. 简单接触 GDB , 开始调试 r n p**

第一个演示代码 heoo.c

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

```
#include 
<
stdio.h
>
int
 g_var = 
0
;


static
int
 _add(
int
 a, 
int
 b) {
    printf(
"
_add callad, a:%d, b:%d\n
"
, a, b);
    
return
 a+
b;
}


int
 main(
void
) {
    
int
 n = 
1
;
    
    printf(
"
one n=%d, g_var=%d\n
"
, n, g_var);
    
++
n;
    
--
n;
    
    g_var 
+= 
20
;
    g_var 
-= 
10
;
    n 
= _add(
1
, g_var);
    printf(
"
two n=%d, g_var=%d\n
"
, n, g_var);
    
    
return
0
;
}
```

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

我们下面从图说起.\(如果用视频说更好,文字和图意义在于查询方便.更简约\)

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416131651535-1150983917.png)

第一个命令 gdb heoo.out 表示 gdb加载 heoo.out 开始调试. 如果需要使用gdb调试的话编译的时候 gcc 需要加上 -g命令.

其中l命令表示 查看加载源码内容. 下面将演示如何加断点.

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416131903348-1338555342.png)

r 表示调试的程序开始运行.

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416132009926-1891976657.png)

p 命令表示 打印值. n表示过程调试, 到下一步. 不管子过程如何都不进入. 直接一次跳过.

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416132131379-608674774.png)

上面用的s 表示单步调试, 遇到子函数,会进入函数内部调试.

总结一下 .** l 查看源码 , b 加断点, r 开始运行调试, n 下一步, s下一步但是会进入子函数. p 输出数据.**

到这里gdb 基本会用了. 是不是也很容易. 直白. 小代码可以随便调试了.

看到这里基础知识普及完毕了. 后面可以不看了. 有机会再看. 好那我们接着扯.



**正文 第一部分 gdb其它开发中用的命令**

开始扯一点, linux总是敲命令操作, 也很不安全. 有时候晕了. 写这样编译命令.

```
gcc -g -Wall -o heoo.c heoo.
out
```

非常恐怖, heoo.c 代码删除了. heoo.out =&gt; heoo.c 先创建后生成失败退出. 原先的内容被抹掉了. 哈哈. 服务器开发, 经验不足, 熟练度不够.自己都怕自己.

**1.  gdb 其它常用命令用法 c q b info**

首先看 用到的调试文件 houge.c

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

```
#include 
<
stdio.h
>

#include 
<
stdlib.h
>

#include 
<
time.h
>
/*

 * arr 只能是数组
 * 返回当前数组长度
 
*/
#define
 LEN(arr) (sizeof(arr)/sizeof(*arr))


//
 简单数组打印函数
static
void
 _parrs(
int
 a[], 
int
 len) {
    
int
 i = -
1
;
    puts(
"
当前数组内容值如下:
"
);

    
while
(++i 
<
 len) 
        printf(
"
%d 
"
, a[i]);    
    putchar(
'
\n
'
);
}


//
 简单包装宏, arr必须是数组
#define
 PARRS(arr) \

    _parrs(arr, LEN(arr))


#define
 _INT_OLD (23)


/*

 * 主函数,简单测试
 * 测试 core文件, 
 * 测试 宏调试
 * 测试 堆栈内存信息
 
*/
int
 main(
void
) {
    
int
 i;
    
int
 a[_INT_OLD];
    
int
* ptr =
 NULL;    

    
//
 来个随机数填充值吧
    srand((unsigned)time(NULL));
    
for
(i=
0
; i
<
LEN(a); ++
i)
        a[i] 
= rand()%
222
;
    
    PARRS(a);

    
//
全员加double, 包含一个错误方便测试
for
(i=
1
; i
<
=LEN(a); ++
i)
        a[i] 
<
<
= 
1
;
    PARRS(a);

    
//
 为了错,强制错

    *ptr = 
0
;

    
return
0
;
}
```

[![](https://common.cnblogs.com/images/copycode.gif "复制代码")](javascript:void%280%29;)

同样需要仔细看下面图中使用的命令. 首先对前言部分加深一些. 看下面

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416133050051-1655357689.png)

这个图是前言的补充, c跳过直到下一个断点处, q表示程序退出.

在 houge.c 中我们开始调试. 一运行段错误, 出现了我们的 core.pid 文件

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416133146816-1224410142.png)

通过 **gdb houge.out core.27047 **开始调试. 马上定位出来了错误原因.

**2. 调试 内存堆栈信息**

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416133619848-1048186703.png)

刚开始 print a , 在main中当做数组处理.打印的信息多. 后面在\_add函数中, a就是个形参数组地址.

主要看** info args  查看当前函数参数值**

**info locals 看当前函数栈上值信息, info registers 表示查看寄存器值.**

后面**查看内存信息** 需要记得东西多一些. 先看图

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416134146957-595254137.png)

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

**3. gdb 设置条件断点**

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416134718488-78631647.png)

很简单 **b 17 if i == 8.** 在17行设置一个断点,并且只有i==8的时候才会触发.

**4. gdb 删除断点**

gdb 删除有**d** 后面跟断点索引1,2,3..

**clear** 行数或名称. 删除哪一行断点. 看下面演示

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416134922801-660565001.png)

到这里 介绍的gdb调试技巧基本都够用了. 感觉用图形ide,例如vs调试也就用到这些了.

估计gdb调试突破20min过去了.够用了.  后面可以不用看了.



**正文 第二部分 gdb 多线程多进程调试**

　　到这里实战中用的机会少了, 也就老鸟会用上些. 这部分可以调试,不好调试. 一般一调估计小半天就走了. 好,那我们处理最后10min.

**1. 首先对上面正文第一部分加深 gdb调试宏**

![](https://images2015.cnblogs.com/blog/532523/201604/532523-20160416140413785-1248548547.png)

首先看上面命令编译命令

```
gcc -Wall -g -o dasheng.
out
 dasheng.c -lpthread编译命令

gcc -Wall -g -o dasheng.out dasheng.c -lpthread
```

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

**2. 开始多线程调试**

首先看测试用例 dasheng.c

按 Ctrl+C 复制代码

按 Ctrl+C 复制代码

编译命令

```
gcc -Wall -g -o dasheng.
out
 dasheng.c -lpthread
```



