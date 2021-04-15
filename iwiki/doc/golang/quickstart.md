



[toc]

# 环境搭建



## Mac安装Golang

### 下载 & 安装

官网地址：https://golang.google.cn/dl/

默认的安装目录 : `/usr/local/go`

### 配置环境变量

```shell
 export PATH=/usr/local/go/bin:$PATH
```

### 创建项目

#### 项目目录介绍

1. 创建项目的根目录

2. 项目跟目录中分别创建 bin pkg src 三个目录

   + bin 用于存放编译后的可执行文件

   + pkg 用于存放编译后的包文件
   + src  源代码

```
lipanpan@lipanpandeMacBook-Pro gocode % tree -L 1
.
├── README.md
├── bin   
├── pkg
└── src
```

3. 配置系统环境变量 将go编译器的路径添加到环境变量 使用go命令可以直接调用自己编写的源代码 (安装时默认已执行)

```shell
 export PATH=/usr/local/go/bin:$PATH
```

4. GOROOT : Go源码目录，用于调用Go相关源码

```shell
export GOROOT=/usr/local/go
```

5. GOPATH : 个人开发所创建的目录 

```shell
export GOPATH=/Users/lipanpan/code/gocode/
```

6. GOBIN : 项目中 bin 目录 编译后的可执行文件

```shell
export GOPATH=/Users/lipanpan/code/gocode/bin
```

7. 加载环境变量

```shell
source /etc/profile
```

注意：上述的方法为临时修改环境变量，如果想要永久修改环境变量，可以把环境变量添加到 `.bash_profile` 如若没有可以自行创建

```shell
cd 
vim .bash_profile

```

#### 编写Golang代码

编写代码的时候需要在`src`下进行编写

```shell
lipanpan@lipanpandeMacBook-Pro grammer % tree -I "go_pointer|go_map|go_array|go_function|go_constant|go_slice|go_struct|go_condition" -L 5
.
├── go_operation
│   └── main.go
├── go_variable
│   └── main.go
└── quick_start.go

```

编写属于你的第一段go程序

```go
package main

import "fmt"

func main() {
	fmt.Println("人生苦短，Let us go")
}
```

### 运行代码

#### Golang运行代码三种方式

go运行的三种方式分别为  `go run` `go build` `go install` 

##### go run 命令

go run 编译源码，并且执行源码的main函数，不会再当前目录下留下可执行文件

```shell
lipanpan@lipanpandeMacBook-Pro grammer % go run quick_start.go
人生苦短，Let us go
```

##### go build 命令

go build 有多种编译方法，无参数编译，文件列表编译，指定包编译等 

```go
lipanpan@lipanpandeMacBook-Pro grammer % go build quick_start.go 
lipanpan@lipanpandeMacBook-Pro grammer % ll
-rwxr-xr-x  1 lipanpan  staff   1.9M  4 14 23:34 quick_start
-rw-r--r--  1 lipanpan  staff    85B  4 14 23:22 quick_start.go
lipanpan@lipanpandeMacBook-Pro grammer % ./quick_start 
人生苦短，Let us go
```

常用的参数：

 

参考地址 ：go build 的详细使用方式方法请参考该地址 ：http://c.biancheng.net/view/120.html



##### go install 命令























