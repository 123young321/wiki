# 数组
数组是**定长**并且**元素类型一致**的集合

## 声明数组

声明数组共有以下几种方式：
+ 声明并且赋值
+ 声明并初始化
+ 初始化
+ 下标初始化
+ 声明不定长数组
+ 字面量声明
+ 指针数组

### 声明并赋值
先声明，声明的同时在内存中开辟了空间，内存的初始化值为 0

```go
var balance [10] float32 // 数组名 balance 长度为10 类型 float32
fmt.Println(balance) // [0 0 0 0 0 0 0 0 0 0]
```

### 声明并初始化
https://www.bilibili.com/video/BV1Mi4y1x7xF?p=56&spm_id_from=pageDriver
```go

```

### 初始化

```go
var members = [3]string{"kevin","Tom","Lucy"}
fmt.Println(members) // [kevin Tom Lucy]
```

### 下标初始化

```go
cities := [...]string{"beijing","shanghai","shenzhen"}
fmt.Println(cities)
```

### 声明不定长数组

```go
var score = [5] int{1:2,3:4}
fmt.Println(score) // [0 2 0 4 0]
```

### 字面量声明

```go
dbs := [3]string{"mysql","redis","mongo"}
fmt.Println(dbs)
```

### 指针数组
指针类型数组，不会开辟内存初始化数组中的值，numbers = nil 指向的是空指针
```go
var numbers *[3]int
fmt.Println(numbers) // <nil>
```

## 内存管理



