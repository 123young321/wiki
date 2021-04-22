
# for循环

## Go中死循环

在 Go 中没有类似于其他语言的 while 的循环方式，是基于 `for{...}` 中的方式实现

示例：Go 中的死循环
```go

// golang中的死循环
for {
    fmt.Println("for中的死循环")
    time.Sleep(1)
}

// golang中的条件判断循环
flag := true
for flag {
    fmt.Println("Let us go ")
    time.Sleep(time.Second * 1)
}

// for 循环中引用外部引用局部变量
numbers := 1
for numbers < 5 {
    fmt.Println("局部变量的使用")
    numbers = numbers + 1
}
fmt.Println(numbers)


```

## Go 循环的四种形式

+ 变量和条件

示例：循环中引用外部引用局部变量
```go
// 
for numbers := 1；numbers < 5 {
    fmt.Println("局部变量的使用")
    numbers := 1
}
fmt.Println(numbers)

```





