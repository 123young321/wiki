# Map

Map在Go中是一种集合的数据类型，存储方式是基于键值对的方式进行存储，在Python中也叫字典(dict)，Map是一种无序的键值对，它的特点是通过key进行检索数据，key也可以是索引指向数据

示例：map的基本演示
```go
map[
    France:巴黎 
    India :新德里 
    Italy:罗马 
    Japan:东京
]
```
## 声明初始化

### 声明变量

示例：make的声明方式
```go
countryCapitalMap = make(map[string]string)
// 变量的赋值
countryCapitalMap["France"] = "巴黎"
countryCapitalMap["Italy"] = "罗马"
countryCapitalMap["Japan"] = "东京"
countryCapitalMap["India "] = "新德里"
// 在使用make的方式还可以进行容量的设置
countryCapitalMap = make(map[string]string，10)
```

示例：类型推到声明

```go
userinfo := map[string]string{"name": "lee", "age": "18"}
fmt.Println(userinfo)
>>>
map[age:18 name:lee]
```

示例：声明变量,该种方式在实际的工作中并不是经常的用到
```go
// 声明变量 默认变量是 nil
var countryCapitalMap map[string]string
fmt.Println(countryCapitalMap)
// 特别注意该种的声明方式不能进行赋值操作 内部指向的是一个空指针所以不能进行赋值
countryCapitalMap["中国"] = "北京" // 此时程序会报错
// 但是我们可以对其整体的变量的赋值
userinfo := map[string]string{"name": "lee", "age": "18"}
var countryCapitalMap map[string]string
// 可以将 map 的集合 userinfo直接赋值给 countryCapitalMap
```
TODO:增加map内存地址的打印结果

示例：基于 new 的方式创建
```go
// 注意该种方式不能指定key进行赋值，因为在内部返回的依然是 nil
value := new(map[string]int)
value["score"] = 100 // 报错
// 在进行整体赋值的时候，需要注意的是 value 是一个指针类型，那么对应整体赋值的变量也应该是一个指针类型
value = &data // data 表示的是整体赋值的变量
```
## 基本的常用操作

示例：创建一个 map 集合
```go
countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}
```

### 长度和容量
```go
// 获取map的长度
length := len(countryCapitalMap)
fmt.Println(length)
// 在Go中可以设置集合的容量
// 获取map的容量
info := make(map[string]string, 10)
info["name"] = "Lee"
// fmt.Println(cap(info)) // 注意在map中不能通过 cap 获取集合的容量
```
> map是由桶构成，一个桶可以存放8个键值对

### 查看
```go
capital, ok := countryCapitalMap["American"] /*如果确定是真实的,则存在,否则不存在 */
/*fmt.Println(capital) */
/*fmt.Println(ok) */
if ok {
    fmt.Println("American 的首都是", capital)
} else {
    fmt.Println("American 的首都不存在")
}
```
### 删除
```go
/*删除元素*/ delete(countryCapitalMap, "France")
```
### 嵌套
```go
data := make(map[string][2]map[string]string) // data := make(map[string][]map[string]string) 切片的形式返回
data["item"] = [2]map[string]string{map[string]string{"name": "lee", "age": "18"}, map[string]string{"name": "lee", "age": "18"}}
data["item2"] = [2]map[string]string{map[string]string{"name": "lee", "age": "18"}, map[string]string{"name": "lee", "age": "18"}}
fmt.Println(data)
>>>
data = {
    "item": [
        {"name": "lee", "age": "18"},
        {"name": "loo", "age": "19"}
    ],
    "item2": [
        {"name": "lee", "age": "28"},
        {"name": "loo", "age": "29"}
    ]
}
```
### 变量的赋值
```go
data := make(map[string]string)
data["key"] = "value"
data2 := data
data["key"] = "new"
fmt.Println(data,data) // 此时data和data2对应的键值对都发生了改变
```
注意：此时变量在内存中指向的是同一个地址，即使无限扩容也指向同一个地址








