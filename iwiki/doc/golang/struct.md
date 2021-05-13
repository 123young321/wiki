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


