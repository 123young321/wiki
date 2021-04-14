



[toc]

# 环境搭建



## Mac安装Go

### 下载并安装

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

5. GOPATH : 项目中 bin 目录 当我们使用 go install 命令进行代码编译的时候 可执行的文件会生成到这个目录

```shell
export GOPATH=/Users/lipanpan/code/gocode/bin
```

6. 加载环境变量

```shell
source /etc/profile
```

注意：上述的方法为临时修改环境变量，如果想要永久修改环境变量，可以把环境变量添加到 `.bash_profile` 如若没有可以自行创建

```shell
cd 
vim .bash_profile

```

#### 编写Go代码

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
	fmt.Printf("人生苦短，Let us go")
}
```

#### 运行代码



















