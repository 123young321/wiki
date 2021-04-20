# String类型

字符串是一个不可改变的字节序列，Go中的String通常是用来包含人类可读的文本，文本中字符串通常被解释为UTF8编码的Unicode码点，Go的字符串是由单个字节连接起来的



示例：字符串的本质 utf-8编码的序列
```go
// unicode 字符集：文字 -> 码点（ucs4 4个字节表示）
// utf-8 :
var name string = "李盼盼"
// 获取的 name[0] 表示的是字节 230 默认为十进制
fmt.Println(name[0]) // 230
fmt.Println(name[1]) // 157
fmt.Println(name[2]) // 142
// 将获取到的十进制 转换为 二进制 计算机执行的二进制文件
fmt.Println(name[0], strconv.FormatInt(int64(name[0]), 2)) // 11100110
fmt.Println(name[1], strconv.FormatInt(int64(name[1]), 2)) // 10011101
fmt.Println(name[2], strconv.FormatInt(int64(name[2]), 2)) // 10001110
// 字符串的长度 9 : 表示的是 utf-8编码的字节长度
fmt.Println(len(name))

```





## 基本操作



示例：字符串不可改变

```go


```


示例：字符串的转义字符

```go

```


示例：字符串的拼接



示例：字符串的索引



示例：字符串的遍历

```go

```



## strings

