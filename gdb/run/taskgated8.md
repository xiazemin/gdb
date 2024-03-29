在初次使用 gdb 时，可能会遇到这样的错误：

```
(gdb) r
Starting program: /Users/didi/PhpstormProjects/c/c/gdb/e e
Unable to find Mach task port for process-id 90315: (os/kern) failure (0x5).
 (please check gdb is codesigned - see taskgated(8))
(gdb) q
```

这是因为 Darwin 内核在你没有特殊权限的情况下，不允许调试其它进程。调试某个进程，意味着你对这个进程有完全的控制权限，所以为了防止被恶意利用，它是默认禁止的。允许 gdb 控制其它进程最好的方法就是用系统信任的证书对它进行签名。

## 创建证书 {#articleHeader0}

按入下步骤创建代码签名的证书：

1. 打开 Keychain Access 应用程序（/Applications/Utilities/Keychain Access.app）

2. 执行菜单**钥匙串访问**-&gt;**证书助理**-&gt;**创建证书**

3. 填写如下信息：

   * 名称：gdb\_codesign

   * 身份类型：自签名根证书

   * 证书类型：代码签名

   * 钩选：**让我覆盖这些默认设置**

一路确定，直到**指定证书位置**的步骤，选择**系统**

1. 点击“创建”，会提示用输入系统登录密码，创建完成

2. 在**钥匙串访问程序**中，选择左侧栏的**系统**和**我的证书**，找到你刚刚创建的**gdb\_codesign**证书并双击打开证书信息窗口，展开**信任**项，设置**使用此证书时：**为**始终信任**。

3. 关闭**证书信息**窗口，系统会再次要求输入系统登录密码

## 对 gdb 签名 {#articleHeader1}

执行下面的命令：

```
codesign
-s gdb_codesign gdb
```

执行上面的命令时，系统会再次验证身份。  
完成后一定要重启系统，这个很重要，否则签名不会生效。

如果出现下面的错误：

> MacBook:~ sam$ codesign -s gdb\_codesign gdb  
> gdb: No such file or directory

那么就指定 gdb 的全路径。

```
$codesign -s gdb_codesign gdb
gdb: No such file or directory
17:09:48-didi@localhost:~/PhpstormProjects/c/c/gdb$which gdb
/usr/local/bin/gdb
17:10:08-didi@localhost:~/PhpstormProjects/c/c/gdb$codesign -s gdb_codesign /usr/local/bin/gdb
17:10:27-didi@localhost:~/PhpstormProjects/c/c/gdb$
```

一定要重启才能生效

