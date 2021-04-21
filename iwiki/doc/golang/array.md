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

## 数组的内存管理

数组的内存是连续的
数组的内存地址实际上是第一个元素地址
字符串存储的是指针地址和字节的长度
示例：字符串源码
```go


```
示例：输出内存地址 `%p`
```go
nums8 := [3]int8{11, 22, 33}
fmt.Printf("数组的内存地址：%p \n", &nums8)
fmt.Printf("数组的第一个元素的内存地址：%p \n", &nums8[0])
fmt.Printf("数组的第二个元素的内存地址：%p \n", &nums8[1])
fmt.Printf("数组的第三个元素的内存地址：%p \n", &nums8[2])

nums32 := [3]int32{12, 13, 14}
fmt.Printf("数组的内存地址：%p \n", &nums32)
fmt.Printf("数组的第一个元素的内存地址：%p \n", &nums32[0])
fmt.Printf("数组的第二个元素的内存地址：%p \n", &nums32[1])
fmt.Printf("数组的第三个元素的内存地址：%p \n", &nums32[2])

>>>

数组的内存地址：0xc0000ae002 
数组的第一个元素的内存地址：0xc0000ae002 
数组的第二个元素的内存地址：0xc0000ae003 
数组的第三个元素的内存地址：0xc0000ae004 
数组的内存地址：0xc0000ae010 
数组的第一个元素的内存地址：0xc0000ae010 
数组的第二个元素的内存地址：0xc0000ae014 
数组的第三个元素的内存地址：0xc0000ae018 

```

## 数组的可变和拷贝

+ 数组的元素可以被更改，但是长度和类型不能更改
示例：更改数组中的元素
```go
nameArray := [2]string{"大湿胸", "小师妹"}
// 更改数组内部的元素
nameArray[1] = "小湿妹"
fmt.Println(nameArray)
```




