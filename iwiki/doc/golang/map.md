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


TODO:增加map内存地址的打印结果

示例：make的声明方式
```go
countryCapitalMap = make(map[string]string)
// 变量的赋值


```

示例：类型推到声明
```go
userinfo := map[string]string{"name": "lee", "age": "18"}
fmt.Println(userinfo)
>>>
map[age:18 name:lee]
```
示例：声明变量
```go
// 声明变量 默认变量是 nil
var countryCapitalMap map[string]string
fmt.Println(countryCapitalMap)
// 特别注意该种的声明方式不能进行赋值操作 内部指向的是一个空指针所以不能进行赋值
countryCapitalMap["中国"] = "北京" // 此时程序会报错
// 但是我们可以对其整体的变量的赋值
data := 


>>>

```







