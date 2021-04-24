# 切片


切片就是动态数组

切片是Go中重要的数据类型，每个切片对象内部都维护着：数组指针，切片长度，切片容量三个数据
```go
type slice struct {
	array unsafe.Pointer
	let int
	cap int
}
```

在向切片中追加个数大于容量时，内部会自动扩容且每次扩容都当容量的2倍（当容量超过 1024 时每次扩容则只增加 1/4 容量）
示例：切片源码分析
```go
// src/runtime/slice.go
newcap := old.cap
doublecap := newcap + newcap
if cap > doublecap {
    newcap = cap
} else {
    if old.cap < 1024 {
        newcap = doublecap
    } else {
        // Check 0 < newcap to detect overflow
        // and prevent an infinite loop.
        for 0 < newcap && newcap < cap {
            newcap += newcap / 4
        }
        // Set newcap to the requested cap when
        // the newcap calculation overflowed.
        if newcap <= 0 {
            newcap = cap
        }
    }
}
```
## 创建切片

创建切片有不同的方式

示例：声明切片
```go
var nums []int // 声明切片
fmt.Println(nums)  // []
```

示例：声明并赋值
```go
var data = []int{1, 2, 3}
fmt.Println(data) // [1,2,3]
```

示例：基于make创建
```go
var users = make([]int, 2, 5) // 切片类型 内部初始化2个数据 容量5 
fmt.Println(users) // [0 0]
```
注意：make 只是用于 切片、字典、channel 的方式创建

示例：基于new创建返回的是指针类型

```go
// rooms 为指针类型 指针指向的是一个切片（数组指针，数组长度，容量长度）数组指针指向的是数组
var rooms = new([]int) // 返回的是一个数组指针
// 指针类型 nil
var foo *[]int // 返回的是一个nil空指针
```

## 自动扩容

示例：切片的内部基本数据
```go
var roles = make([]int, 1, 3)
fmt.Printf("切片的内部结构roles：%v,切片的长度：%d,切片的容量：%d", roles, len(roles), cap(roles))
var full = make([]int,3)
fmt.Println()
fmt.Printf("切片的内部结构fules：%v,切片的长度：%d,切片的容量：%d", full, len(full), cap(full))
>>>
切片的内部结构roles：[0],切片的长度：1,切片的容量：3
切片的内部结构fules：[0 0 0],切片的长度：3,切片的容量：3
```
> [!注意]
> 当不初始化长度的时候此时的长度为容量


在Go中通过`append的方法可以在数组中插入一个数据`此时会在内部返回一个新的切片，返回新的切片内存地址会和扩容之前的地址指向同一个内存地址
```go
 cap=3
 len=1
 array=0xc000122028
roles1 --------
                \
                 +-----------+
                 | 0 |   |   | ----> role2 := append(roles1,8)
                 +-----------+  
                / 内存地址：0xc000122028 
roles2 --------
 cap=3
 len=2 // 注意通过append的方插入新的数据此时长度变为 2
 array=0xc000122028
```
示例：扩容后的内存地址会和之前指向同一个内存地址,修改源切片此时扩容后的切片也会变化
```go


```