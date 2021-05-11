
# 指针

指针是一种数据类型，用于表示数据的内存地址

```go
// 声明一个字符串类型的变量，默认的初始值为 空 字符串
var str string 
// 声明一个字符串的指针类型 默认初始值为 nil
var point *string
```

示例：指针类型的赋值操作 `&变量名`
```go
// 声明一个 变量 并 赋值
var name string = "Kevin"
// 声明一个指针类型存储的是 name 的对应的内存地址
var point *string = &name
```

## 变量的内存地址 & 变量名

示例：获取变量的内存地址 `&a`

```go
var a int = 10
fmt.Printf("变量的内存地址：%x\n", &a) // 变量的内存地址：c00001c0a8
```

## 指针变量取值操作 

示例：指针变量取值操作 `*b`
```
var b := &a // b此时存储的是a的内存地址，如果想通过内存地址获取到值通过 *
// 指针取值
fmt.Printf("*b 变量的值: %d\n", *b)

```
## 指针的使用场景
示例：传递值和传递指针
```go

func changeName(name string) {
	name = "other name"
}

func changeNamePointer(pre *string) {
	*pre = "other name"
}

func main() {
	name := "lee"
	// 本质会将name的值拷贝
	changeName(name)
	fmt.Println(name) // lee
	changeNamePointer(&name)
	fmt.Println(name) // other name
}
```
示例：
```go
package main

import "fmt"

func main() {
	var username string
	fmt.Printf("请输入用户名： ")

	fmt.Scanf("%s", &username)

	if username == "lee" {
		fmt.Println("登录成功")
	} else {
		fmt.Println("登录失败")
	}
}
```

## 指针的指针

示例：指针的指针
```go
name := "Lee"
// 声明一个指针类型的变量 内部存储name内存地址
var p1 *string = &name
// 声明一个指针类型的变量 p2 内部存储p1的变量
var p2 **string = &p1
// 声明一个指针类型的变量 p3 内部存储p2的变量
var p3 ***string = &p2
fmt.Println(name, &name)
fmt.Println(p1, &p1)
fmt.Println(p2, &p2)
fmt.Println(p3, &p3)
```
示例：指针的指针重置值的操作
```go
name := "Lee"

// 声明一个指针类型的变量p1，内部存储name的内存地址
var p1 *string = &name
*p1 = "Lee2" // 将name的值 Lee改为 Lee2

// 声明一个指针的类型存储的是 p1 的内存地址
var p2 **string = &p1
**p2 = "Lee3" // 将 name 中内存的值改 Lee3

var p3 ***string = &p2
***p3 = "Lee4" // 将name的值改 Lee4

fmt.Printf("name中存储的值 %s name存储的内存地址%p \n", name, &name)
fmt.Printf("p1中存储的的值%p,p1的内存地址为%p \n", p1, &p1)
fmt.Printf("p2中存储的的值%p,p2的内存地址为%p \n", p2, &p2)
fmt.Printf("p3中存储的的值%p,p3的内存地址为%p \n", p3, &p3)
>>>
name中存储的值 Lee4 name存储的内存地址0xc000096220 
p1中存储的的值0xc000096220,p1的内存地址为0xc0000b2018 
p2中存储的的值0xc0000b2018,p2的内存地址为0xc0000b2020 
p3中存储的的值0xc0000b2020,p3的内存地址为0xc0000b2028 
```
## 指针的运算
