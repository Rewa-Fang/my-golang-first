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