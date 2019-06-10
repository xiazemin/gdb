[https://golang.org/doc/gdb](https://golang.org/doc/gdb)

已经有许多

[Go 的调试器](https://github.com/mailgun/godebug)

存在了，其中一些调试器的不好之处是通过在编译时注入代码来提供一个交互终端。gdb 调试器则允许你调试已经编译好的二进制文件，只要他们已经与 debug 信息连接，并不用修改源代码。这是个相当不错的特性，因此你可以从你的部署环境中取一个产品然后灵活地调试它。你可以从

[Golang 官方文档](https://golang.org/doc/gdb)

中阅读更多关于 gdb 的信息

```
localhost:go_http didi$ go build -gcflags "-N -l" -o go_http main.go 
localhost:go_http didi$ ls
debug   go_http main    main.go
localhost:go_http didi$ gdb go_http 
GNU gdb (GDB) 7.12.1
Copyright (C) 2017 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-apple-darwin15.6.0".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
---Type <return> to continue, or q <return> to quit---return
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from go_http...done.
Loading Go Runtime support.
(gdb) source /usr/local/src/go/src/runtime/runtime-gdb.py
/usr/local/src/go/src/runtime/runtime-gdb.py: No such file or directory.
(gdb) pwd
Working directory /Users/didi/goLang/src/go_http.
(gdb) source  /Users/didi/goLang/src/runtime-gdb.py
.DS_Store           consul/             git.apache.org/     goWeb/              gopkg.in/           livego/             pool/               turning/
EasyProxy/          didicar.sql         github.com/         go_http/            goplay/             minididi/           pprof/              websocket/
GoDesignPattern/    docker-1.14.0/      go/                 goccgo/             groupOne/           minididi_20/        proxy/              
bitcoin/            examples/           go.etcd.io/         golang.org/         helloBee/           nanogui-go/         simple_server       
code.google.com/    formatHtml/         go1.7.3.src.tar.gz  goobj_explorer/     koala/              owl/                sourcegraph.com/    
(gdb) source  /Users/didi/goLang/src/runtime-gdb.py
.DS_Store           consul/             git.apache.org/     goWeb/              gopkg.in/           livego/             pool/               turning/
EasyProxy/          didicar.sql         github.com/         go_http/            goplay/             minididi/           pprof/              websocket/
GoDesignPattern/    docker-1.14.0/      go/                 goccgo/             groupOne/           minididi_20/        proxy/              
bitcoin/            examples/           go.etcd.io/         golang.org/         helloBee/           nanogui-go/         simple_server       
code.google.com/    formatHtml/         go1.7.3.src.tar.gz  goobj_explorer/     koala/              owl/                sourcegraph.com/    
(gdb) source  /Users/didi/goLang/src/github.com/goruntime-gdb.py
go-sql-driver/ go-yaml/       gocode/        goji/          golang/        golang-china/  gorilla/       
(gdb) source  /Users/didi/goLang/src/github.com/golang/runtime-gdb.py
glog/     go/       lint/     protobuf/ snappy/   sys/      tools/    tour/     
(gdb) source  /Users/didi/goLang/src/github.com/golang/go/runtime-gdb.py
.git/            .github/         AUTHORS          CONTRIBUTORS     PATENTS          api/             favicon.ico      misc/            src/
.gitattributes   .gitignore       CONTRIBUTING.md  LICENSE          README.md        doc/             lib/             robots.txt       test/
(gdb) source  /Users/didi/goLang/src/github.com/golang/go/src/runtime-gdb.py
Make.dist         buildall.bash     compress/         expvar/           internal/         mime/             race.bat          strconv/          unsafe/
all.bash          builtin/          container/        flag/             io/               naclmake.bash     reflect/          strings/          vendor/
all.bat           bytes/            context/          fmt/              iostest.bash      nacltest.bash     regexp/           sync/             
all.rc            clean.bash        crypto/           go/               log/              net/              run.bash          syscall/          
androidtest.bash  clean.bat         database/         hash/             make.bash         os/               run.bat           testing/          
archive/          clean.rc          debug/            html/             make.bat          path/             run.rc            text/             
bootstrap.bash    cmd/              encoding/         image/            make.rc           plugin/           runtime/          time/             
bufio/            cmp.bash          errors/           index/            math/             race.bash         sort/             unicode/          
(gdb) source  /Users/didi/goLang/src/github.com/golang/go/src/runruntime-gdb.py
run.bash  run.bat   run.rc    runtime/  
(gdb) source  /Users/didi/goLang/src/github.com/golang/go/src/runtime/runtime-gdb.py
Loading Go Runtime support.
(gdb) l
1       package main
2       
3       import (
4               "io"
5               "net/http"
6       )
7       
8       func hello(rw http.ResponseWriter, req *http.Request) {
9               io.WriteString(rw, "hello world")
10      }
(gdb) l 15
10      }
11      func main() {
12              print("hello world")
13              http.HandleFunc("/hello/world", hello)
14              http.ListenAndServe(":8001", nil)
15      }
(gdb) b main.go:12
Breakpoint 1 at 0x120ea71: file /Users/didi/goLang/src/go_http/main.go, line 12.
(gdb) r
Starting program: /Users/didi/goLang/src/go_http/go_http 
[New Thread 0x141b of process 42016]
[New Thread 0x1503 of process 42016]
[New Thread 0x1603 of process 42016]
[New Thread 0x1703 of process 42016]

Thread 1 hit Breakpoint 1, main.main () at /Users/didi/goLang/src/go_http/main.go:12
12              print("hello world")
(gdb)
```

方式一：在.gdbinit文件中添加如下配置,没有文件就创建一个,重启gdb生效

`add-auto-load-safe-path /home/gws/go/gosrc/go/src/runtime/runtime-gdb.py`

方式二：

```
(gdb) source /usr/local/src/go/src/runtime/runtime-gdb.py
```

```

```



