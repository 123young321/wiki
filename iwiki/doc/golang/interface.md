# 接口

## 定义接口
```
type 接口名称 interface {
  方法名 返回值
}
```
示例：
```
type Base interface {
	f1()                   // 定义方法
	f2() int               // 定义方法 返回int类型
	f3() (int bool)        // 定义方法 2个返回值 int bool类型
	f4(n1 int, n2 int) int // 定义方法 需要两个参数 一个返回值
}

type empty interface{} // interface
```
接口的方法只是定义，不能实现具体的逻辑
## 接口的作用
在程序中接口一般有两大作用代指类型和约束

### 空接口

空接口代指任意类型

示例：
```
package main

import "fmt"


// 定义一个空接口
type Base2 interface{}

func main() {
	// 定义一个切片，内部可以存放任意类型
	dataList := make([]Base2, 0)
	// 切片中添加字符串类型
	dataList = append(dataList, "string")
	// 切片中添加整形
	dataList = append(dataList, 18)
	// 切片中添加 浮点型
	dataList = append(dataList, 99.99)
	fmt.Println(dataList)
}

```
示例2：
```
package main

import "fmt"

type Person struct {
	name string
	age  int
}

func something(arg interface{}) {
	fmt.Println(arg)
}

func main() {

	something("String")                       // 传入的是字符串
	something(666)                            // 传入的是整形
	something(Person{name: "kevin", age: 19}) // 传入的是结构体

}
```
由于接口只是代指这些数据类型(在内部其实是转换了接口类型)，想要在获取数据中的值时，需要再将接口转换为指定的数据类型
```
package main

import "fmt"

type Person struct {
	name string
	age  int
}

func something(arg interface{}) {
	// 接口转换为Person成功，ok=True;否则ok=False
	fmt.Println(arg)
	tp, ok := arg.(Person)
	if ok {
		fmt.Println(tp.name, tp.age)
	} else {
		fmt.Println("转换失败")
	}
}

func main() {
	something(Person{name: "kevin", age: 19}) // 传入的是结构体
}

```
示例：当转换的类型比较多的时候，多个类型的转换，将arg接口对象转换为
```
package main

import "fmt"

type PersonV2 struct {
	name string
	age  int
}

type Role struct {
	title string
	count int
}

func somethingV2(arg interface{}) {
	// 接口转换为Person成功，ok=True;否则ok=False
	fmt.Println(arg)
	switch tp := arg.(type) {
	case PersonV2:
		fmt.Println(tp.name)
	case Role:
		fmt.Println(tp.title)
	case string:
		fmt.Println(tp)
	default:
		fmt.Println(tp)
	}

}

func main() {

	somethingV2("String")                         // 传入的是字符串
	somethingV2(666)                              // 传入的是整形
	somethingV2(3.14)                             // 传入浮点型
	somethingV2(PersonV2{name: "kevin", age: 19}) // 传入的是结构体
	somethingV2(Role{title: "Admin", count: 2})
}


```

### 非空接口

规范 && 约束

一般定义非接口，都是用于约束结构体中必须含有的方法，例如：

```
package main

import "fmt"

// 定义接口

type IBase interface {
	f1() int
}

// 定义结构体
type PersonV3 struct {
	name string
}

// 为结构体PersonV3定义方法
func (p PersonV3) f1() int {
	return 123
}

// 定义结构体 User
type User struct {
	name string
}

// 为结构体User定义方法
func (p User) f1() int {
	return 666
}

// 基于接口的参数，可以实现传入多种类型(多态), 也同时具有约束对象必须实现接口方法的功能

func DoSomething(base IBase) {
	result := base.f1()
	fmt.Println(result)
}

func main() {
	per := PersonV3{"admin"}
	user := User{name: "root"}
	DoSomething(per)
	DoSomething(user)
}


```

示例：基于接口实现

```
package main

import "fmt"

// 定义接口

type IBaseV2 interface {
	f1() int
}

// 定义结构体
type PersonV4 struct {
	name string
}

// 为结构体PersonV3定义方法
func (p *PersonV4) f1() int {
	return 123
}

// 定义结构体 User
type UserV2 struct {
	name string
}

// 为结构体User定义方法
func (p *UserV2) f1() int {
	return 666
}

// 基于接口的参数，可以实现传入多种类型(多态), 也同时具有约束对象必须实现接口方法的功能

func DoSomethingV2(base IBaseV2) {
	result := base.f1()
	fmt.Println(result)
}

func main() {
	per := &PersonV4{"admin"}
	user := &UserV2{name: "root"}
	DoSomethingV2(per)
	DoSomethingV2(user)
}


```
