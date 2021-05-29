# 结构体

结构体是一个复合数据类型，用于表示一组数据类型，结构体由于一系列属性组成，每个属性都有自己的类型和值


## 定义

### 定义的基本格式

示例：定义结构体语法
```go
type 结构体名称 struct {
    字段 类型
    ...
}
```
### 定义结构体的三种方式

示例1：定义结构体
```go
type Books struct {
	title   string
	author  string
	subject string
	id      int
}
```
示例2：将相同的字符进行同时定义
```go
type Books struct {
	title, author，subject string
	id      int
}
```
示例3：
```go
// 书籍的种类
type Category struct {
	name string
	id   int
}
// 书籍的基本属性
type Books struct {
	title   string
	author  string
	subject string
    category Category
	id      int
}
```
示例4：匿名定义
```go
// 书籍的种类
type Category struct {
	name string
	id   int
}
// 书籍的基本属性
type Books struct {
	title   string
	author  string
	subject string
	id      int
    Category
}
```


## 结构体的基本操作

示例：结构体的初始化
```go
// 初始化一个结构体
fmt.Println(Books{"Go语言", "www.baidu.com", "Go语言教程", 1})
// 也可以使用 key => value 格式
fmt.Println(Books{title: "Golan", author: "Golan author", subject: "Go语言教程", id: 2})
// 忽略的字段为0或者为空 {Go语言的测试版本 David  0}
fmt.Println(Books{title: "Go语言的测试版本", author: "David"})
```

示例：结构体的基本操作
```go
// 声明结构体
var GoBook Books
// 结构体的操作
GoBook.title = "GoBook"
GoBook.author = "Kevin"
GoBook.subject = "Go 语言的教程"
GoBook.id = 2
```

示例：结构体的取值
```go
fmt.Printf("GoBook  title : %s\n", GoBook.title)
fmt.Printf("GoBook  author : %s\n", GoBook.author)
fmt.Printf("GoBook  subject : %s\n", GoBook.subject)
fmt.Printf("GoBook   id : %d\n", GoBook.id)

```

## 结构体指针

```golang
// 创建结构体

type Person struct {
	name string
	age  int
}

func main() {
	//初始化结构体 创建一个结构体对象
	p1 := Person{"Kevin", 18}
	fmt.Println(p1.name, p1.age)
	// 初始化结构体指针
	// var p2 *Person = &Person{"Kevin2", 18}
	p2 := &Person{"Kevin2", 18}
	/**
	此时p2存储的是p1的内存地址
	 */
	fmt.Println(p2.name, p2.age)
	var p3 *Person = new(Person) // 此时返回的 nil 空指针

	p3.name = "Kevin3"
	p3.age = 20
	fmt.Println(p3.name, p3.age)
}

```
## 结构体赋值

应用场景：实现实例的创建

```
type Person2 struct {
	name string
	age  int
}

// 结构体 赋值拷贝
p1 := Person2{name: "Kevin", age: 19}
p2 := p1

fmt.Println(p1) // {Kevin 19}
fmt.Println(p2) // {Kevin 19}

// 修改 p1 的name
p1.name = "Lisa"
fmt.Println(p1) // {Lisa 19}
fmt.Println(p2) // {Kevin 19}

```

## 结构体指针赋值

应用场景：当修改某个结构体的时候，实现所有的数据进行同步

```
func main() {
	// 结构体 指针 赋值
	p3 := &Person2{name: "Kevin3", age: 18}
	p4 := p3
	fmt.Println(p3)
	fmt.Println(p4)
	// 修改结构体 P4 则对应的p3也发生了变化
	p3.name = "Kevin4"
	fmt.Println(p3)
	fmt.Println(p4)
}
>>>
&{Kevin3 18}  // 此时输出的是结构体指针对象
&{Kevin3 18}
&{Kevin4 18}
&{Kevin4 18}
```
## 结构体的赋值拷贝


在存在结构体嵌套时候，赋值会拷贝一份所有的数据
