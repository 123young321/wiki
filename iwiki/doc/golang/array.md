# 数组
数组是**定长**并且**元素类型一致**的集合

## 声明数组

声明数组共有以下几种方式：
+ 声明并且赋值
+ 初始化
+ 下标初始化
+ 声明不定长数组

### 声明并赋值
先声明，声明的同时在内存中开辟了空间，内存的初始化值为 0

```go
var balance [10] float32 // 数组名 balance 长度为10 类型 float32
fmt.Println(balance) // [0 0 0 0 0 0 0 0 0 0]
```

### 初始化
```go
var members = [3]string{"kevin","Tom","Lucy"}
fmt.Println(members) // [kevin Tom Lucy]
```

### 下标初始化
```go

```
### 声明不定长数组
```go

```




