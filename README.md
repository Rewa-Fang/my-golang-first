### my golang first demo

搞了半天终于明白了 GOPATH 与 GOROOT 的配置；

修改环境变量GOPATH的值为你开发GO的项目路径
可以是随意路径，但不可以与GOROOT一样；GOROOT环境变量的值是GO的安装目录；

除此之外： GOPATH路径下必须有以下三个子目录：
我的电脑是E:\myGoPath:
* src -- 存放源代码（比如：.go .c .h .s等）
* bin -- 编译后生成的可执行文件（为了方便，可以把此目录加入到 $PATH 变量中）
* pkg -- 编译后生成的文件（比如：.a）   


OK; 建立好开发目录后。  

开始一个《Go web 编程》上的测试小例子：  

src下建立一个文件夹 `\mymath`  编写`sqrt.go`    
code: 

    package mymath

    func Sqrt(x float64) float64{
        z := 0.0
        for i := 0; i < 1000 ; i++ {
            z -= ( z*z - x) / (2*x)
        }
        return z
    }
   
执行： `go install`  

在任意的目录执行如下代码`go install mymath`  

安装好了之后会在pkg\windows_amd64下会显示 `mymath.a`  

同样再建立一个应用程序来调用：  `src\mathapp\main.go`

        package main import (
        "mymath"
        "fmt"
        )
        func main() {
        fmt.Printf("Hello, world. Sqrt(2) 

  
进入该应用目录，然后执行`go build`  

输入：`mathapp`  

会显示 ：Hello, world. Sqrt(2) = 1.414213562373095 

然后还要安装该应用；在该目录下执行`go install`  

安装成功后/bin/下增加了一个可执行文件`mathapp`  

输入命令：`mathapp`

然后将会显示：
`Hello, world. Sqrt(2) = 1.414213562373095`    

第一个小例子跑起来；GO才刚刚开始。

**路漫漫其修远兮 吾将上下而求索！！！**


### 命令
go build

这个命令主要用于测试编译。在包的编译过程中，若有必要，会同时编译与之相关联的包。
如果是普通包，就像我们在之前编写的mymath包那样，当你执行go build之后，它不会产生任何文件。
如果你需要在%GOPATH%/pkg下生成相应的文件，那就得执行go install了。
如果是main包，当你执行go build之后，它就会在当前目录下生成一个可执行文件。如果你需要在
$GOPATH/bin下生成相应的文件，需要执行go install，或者使用go build -o 路径/a.exe。
如果某个项目文件夹下有多个文件，而你只想编译某个文件，就可在go build之后加上文件名，例如go
build a.go；go build命令默认会编译当前目录下的所有go文件。
你也可以指定编译输出的文件名。例如1.2节中的mathapp应用，我们可以指定go build -o
astaxie.exe，默认情况是你的package名(非mmain包)，或者是第一个源文件的文件名(main包)。
go build会忽略目录下以“_”或“.”开头的go文件。
如果你的源代码针对不同的操作系统需要不同的处理，那么你可以根据不同的操作系统后缀来命名文件。例
如有一个读取数组的程序，它对于不同的操作系统可能有如下几个源文件：arraylinux.go   arraydarwin.go   arraywindows.go   arrayfreebsd.go，go build的时候会选择性地编译以系统名结尾的文件（linux、darwin、windows、freebsd）。例如Linux系
统下面编译只会选择array_linux.go文件，其它系统命名后缀文件全部忽略
go clear
这个命令是用来移除当前源码包里面编译生成的文件。这些文件包括

_obj/ 旧的object目录，由Makefiles遗留
_test/ 旧的test目录，由Makefiles遗留
_testmain.go 旧的gotest文件，由Makefiles遗留
test.out 旧的test记录，由Makefiles遗留
build.out 旧的test记录，由Makefiles遗留
*.[568ao] object文件，由Makefiles遗留

DIR(.exe) 由go build产生
DIR.test(.exe) 由go test -c产生
MAINFILE(.exe) 由go build MAINFILE.go产生

我一般都是利用这个命令清除编译文件，然后github递交源码，在本机测试的时候这些编译文件都是和系统相关的，
但是对于源码管理来说没必要

go fmt
有过C/C++经验的读者会知道,一些人经常为代码采取K&R风格还是ANSI风格而争论不休。在go中，代码则有标准的风
格。由于之前已经有的一些习惯或其它的原因我们常将代码写成ANSI风格或者其它更合适自己的格式，这将为人们在
阅读别人的代码时添加不必要的负担，所以go强制了代码格式（比如左大括号必须放在行尾），不按照此格式的代码
将不能编译通过，为了减少浪费在排版上的时间，go工具集中提供了一个go fmt命令 它可以帮你格式化你写好的代
码文件，使你写代码的时候不需要关心格式，你只需要在写完之后执行go fmt <文件名>.go，你的代码就被修改成
了标准格式，但是我平常很少用到这个命令，因为开发工具里面一般都带了保存时候自动格式化功能，这个功能其实
在底层就是调用了go fmt。接下来的一节我将讲述两个工具，这两个工具都自带了保存文件时自动化go fmt功能。

使用go fmt命令，更多时候是用gofmt，而且需要参数-w，否则格式化结果不会写入文件。gofmt -w src，可以格式
化整个项目。

go get
这个命令是用来动态获取远程代码包的，目前支持的有BitBucket、GitHub、Google Code和Launchpad。这个命令在
内部实际上分成了两步操作：第一步是下载源码包，第二步是执行go install。下载源码包的go工具会自动根据不
同的域名调用不同的源码工具，对应关系如下：
BitBucket (Mercurial Git)
GitHub (Git)
Google Code Project Hosting (Git, Mercurial, Subversion)
Launchpad (Bazaar)
所以为了go get 能正常工作，你必须确保安装了合适的源码管理工具，并同时把这些命令加入你的PATH中。其实
go get支持自定义域名的功能，具体参见go help remote。

 

go install
这个命令在内部实际上分成了两步操作：第一步是生成结果文件(可执行文件或者.a包)，第二步会把编译好的结果移
到$GOPATH/pkg或者$GOPATH/bin

 

go test
执行这个命令，会自动读取源码目录下面名为*_test.go的文件，生成并运行测试用的可执行文件。输出的信息类
似
ok archive/tar 0.011s
FAIL archive/zip 0.022s
ok compress/gzip 0.033s
… 默认的情况下，不需要任何的参数，它会自动把你源码包下面所有test文件测试完毕，当然你也可以带上参数，详情
请参考go help testflag

 

go doc
很多人说go不需要任何的第三方文档，例如chm手册之类的（其实我已经做了一个了，chm手册），因为它内部就有一
个很强大的文档工具。
如何查看相应package的文档呢？ 例如builtin包，那么执行go doc builtin 如果是http包，那么执行go doc
net/http 查看某一个包里面的函数，那么执行godoc fmt Printf 也可以查看相应的代码，执行godoc -src
fmt Printf
通过命令在命令行执行 godoc -http=:端口号 比如godoc -http=:8080。然后在浏览器中打开
127.0.0.1:8080，你将会看到一个golang.org的本地copy版本，通过它你可以查询pkg文档等其它内容。如果你设
置了GOPATH，在pkg分类下，不但会列出标准包的文档，还会列出你本地GOPATH中所有项目的相关文档，这对于经常
被墙的用户来说是一个不错的选择

其它命令
go还提供了其它很多的工具，例如下面的这些工具
go fix 用来修复以前老版本的代码到新版本，例如go1之前老版本的代码转化到go1
go version 查看go当前的版本
go env 查看当前go的环境变量
go list 列出当前全部安装的package
go run 编译并运行Go程序
以上这些工具还有很多参数没有一一介绍，用户可以使用go help 命令获取更详细的帮助信息