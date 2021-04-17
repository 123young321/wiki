

# 变量

Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字，声明变量的一般形式是使用 `var` 关键字：`var 变量名 = 变量值`

## 变量声明

### 基本格式

**声明并赋值**

```go
var name string = "kevin"
```
**先声明后赋值**
```go
var age // 先声明
age = 18
```
**声明多个变量 变量具有相同的数据类型**
```go
var country, city string = "China", "Beijing"
```
**变量的初始化**
```go
var (
	address string
	email   string
)

address = "北京市"
email = "mail@qq.com"
```

**变量的默认值**

- 数值类型  **0**
- 布尔类型为 **false**
- 字符串为 **""**（空字符串）

## 变量简写

### 省略类型

基本格式：`var roomNumber = 6`
```go
var phone, jobNumber = 123456789, "8-1234"
```
### 省略关键字

基本格式：`变量名 := 变量值 `
```go
roomNumber := 666
```
## 全局变量&局部变量

### 全局变量

```go
var address string = "北京"
```
**注意：**
声明全局变量不能省略**数据类型**，**关键字**

```go
// 不能使用以下的声明方式进行声明全局变量
var country = "China"
country := "China"
```
变量首字母大小写
+ 大写 ：任意文件都可以使用该全局变量 （全局）
+ 小写 ：当前包中可以使用 （局部）
### 局部变量

局部变量中有严格的作用域，每个大括号就是一个作用域，每个作用域中都可定义相关的局部变量。
示例：局部变量不能被外部所使用

```go
if city == "北京" {
  add := "朝阳区"
  fmt.Println(add)
}
fmt.Println(add)
// ======= go run =======
./main.go:37:14: undefined: add
```



# 常量

常量和变量唯一不同的就是常量在程序运行的过程中是一个不能被修改的值

## 定义常量

常量的定义方式有两种分别为显示定义和隐式定义

+ 显式定义 ：`const code string = "golang"`

+ 隐式定义 : `const code = "golang"`

当有多个相同类型的声明可以简写为

```go
const code,ide = "go","vscode"
```

## iota

> iota，特殊常量，可以认为是一个可以被编译器修改的常量。

```go
const (
  v1 = 1
  v2 = 2
  v3 = 3
)
fmt.Println(v1, v2, v3)
// ======= go run =======
1 2 3

const (
  v4 = iota
  v5
  v6
)
fmt.Println(v4, v5, v6)
// ======= go run =======
0 1 2
const (
  v7 = iota + 2
  _
  v8
  _
  v9
)
fmt.Println(v7, v8, v9) // 2 4 6
```

上述可以进行拆分



# fmt

## 输出

fmt是 go 中提供用于进行输入、输出的模块，相关的函数有：

+ `fmt.Print` 输出
+ `fmt.Println` 输出并且在末尾添加换行符
+ `fmt.Printf` 格式化输出，常用的占位符
  + `%s`  字符串
  + `%d`  整型
  + `%f`  十进制小数
  + `%2.f`  保留小数点后两位

示例：格式化输出

```go
package main

import "fmt"

func main() {
	
	fmt.Println("个人信息打印")
	fmt.Printf("姓名：%s,年龄：%d", "lipanpan", 18)

}
// ======= go run =======
个人信息打印
姓名：lipanpan,年龄：18
Process finished with exit code 0
```

参考：更多占位符和文档可参考go源码：gitlab地址

示例：动态的输出

```go
// 用户输入并赋值给变量
var val_input string
fmt.Scanf("%s",&val_input)
if val_input == "go" {
	fmt.Println("输入正确")
}else {
	fmt.Println("输入错误")
}
```

## 输入

用户与程序之间的交互

+ `fmt.Scan` ：用户输入函数，等待输入的值满足所定义的要求，例如要求输入两个变量，如果没有输入两个会一直等待输入
+ `fmt.Scanln`  ：等待回车，假设我们需要输入两个变量，如果此时输入回车则表示程序输入结束
+ `fmt.Scanf` ：格式化输入

示例1：`fmt.Scan` 交互输出 

```go
var name string
var age int
fmt.Println("请输入用户名/和年龄空格隔开")
// count 表示输入了几个值 err 表示输入错误返回的报错信息lee
count, err := fmt.Scan(&name, &age)
fmt.Println(count, err)
fmt.Println(name, age)
// ======= go run ======= 输入的值有误
lee w
1 expected integer // 成功的个数 报错信息 如果正确则 err 为 nil
lee 0 // 0 表示的是 int 的默认值
// ======= go run ======= 输入的值正确
	// lee 1
	// 2 <nil>
	// lee 1

// 基于条件判断对返回值进行检查判断
	_, err := fmt.Scan(&name, &age) // 注意此处的下划线
	if err == nil {
		fmt.Println("输入成功")
	} else {
		fmt.Println("输入的信息有误", err)
	}
```

示例2：`fmt.Scanf` 格式化输出 返回值 `count,err`

```go
var info string
fmt.Println("请输入info")
count,err = fmt.Scanf("显示输入的信息%s",&info)// 如果此时输入 显示输入信息xx
fmt.Println(info) // info 返回的是 xx
// 如果不接受count的返回值，可以使用下划线进行展示
_,err = fmt.Scanf("显示输入的信息%s",&info)// 如果此时输入 显示输入信息xx
if err == nil {
  fmt.Println("输入成功")
}else {
  fmt.Println("输入失败",err)
}
```

