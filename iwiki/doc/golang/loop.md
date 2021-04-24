
# 循环

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

## Go 循环的写法

+ 变量和条件
+ 变量&条件&赋值

示例：循环中引用外部引用局部变量

```go
// 
for numbers := 1；numbers < 5; {
    fmt.Println("局部变量的使用")
    numbers := 1
}
fmt.Println(numbers)

```

示例：变量&条件&赋值

```go
for numbers3 := 1; numbers3 < 5;numbers3=numbers3+1 { // numbers3++
    fmt.Println("类似于Javascript中的for循环")
}
// 注意此处不能打印number3的变量
//fmt.Println(numbers3)
```

## break 和 continue 打标签

示例：对于for循环打标签，然后通过break和continue就可以实现多层循环的跳出和终止
```go
// 对于golang中基于break和continue进行打标签
f1:
	for i := 1; i < 5; i++ {
		for j := 1; j < 5; j++ {
			if j == 3 {
				fmt.Println("彻底跳出循环")
				continue f1 // break f1
			}
			fmt.Println(i, j)
		}
	}
}
```

## goto
跳跃到指定的行，然后根据业务逻辑继续执行代码

示例：goto的基本使用

```go
	var name string
	fmt.Println("请输入姓名")
	fmt.Scanln(&name)
	if name == "lee" {
		// SVIP
		goto SVIP
	} else if name == "people" {
		// VIP
		goto VIP
	}

	fmt.Println("预约...")
VIP:
	fmt.Println("等号...")
SVIP:
	fmt.Println("进入...")
	
```















