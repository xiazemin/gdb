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

