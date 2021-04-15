[toc]



# fmt 

fmt是 go 中提供用于进行输入、输出的模块

fmt模块中常见的输出相关函数有：

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



# 变量

Go 语言变量名由字母、数字、下划线组成，其中首个字符不能为数字

声明变量的一般形式是使用 `var` 关键字：`var 变量名 = 变量值`

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

### 变量简写

#### 省略类型 

基本格式：`var roomNumber = 6`

```go
var phone, jobNumber = 123456789, "8-1234"
```

#### 省略关键字

基本格式：`变量名 := 变量值 `

```go
roomNumber := 666
```

### 全局变量&局部变量

#### 全局变量

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

#### 局部变量

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

