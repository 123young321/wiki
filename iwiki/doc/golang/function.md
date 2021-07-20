
# 函数

## 基本语法
```
func 函数名(参数名) 返回值{
  函数体
}
```
定义函数
```
// 定义发送邮件的函数
func SendEmail(email string) bool {
	fmt.Println(email, "你有新的邮件来了")
	return true
}
```
调用函数
```
func main() {
  // 调用函数
  var email string = "123@qq.com"
	result := SendEmail(email)
	if result {
		fmt.Println("发送成功")
	} else {
		fmt.Println("发送失败")
	}
}
```
## 函数的参数

### 多个参数

示例：传递多个参数
```
func add(x, y int) int {
	return x + y
}

```
### 指针参数

```
```
### 函数做参数
```
// 定义函数
func bar3(x, y int, op func(int, int) int) int {
	return op(x, y)
}

// 调用函数
var ret3 = bar3(1, 2, add)
```
### 变长参数
```
// 定义
func bar(x ...int) int {
	println(x) // x 是一个切片 [3/3]0xc00003a760
	sum := 0
	for index, value := range x {
		println(index)
		sum = sum + value
	}
	return sum
}
// 调用
var ret = bar(1, 2, 3)
```
## 函数的返回值

示例：返回值
```
func foo(x string) bool {
	var success bool
	if x == "" {
		return false
	}
	return success
}
```

### 函数多返回值
```
// 定义
func calc(x, y int) (int, int) {
	sum := x + y
	sub := x - y
	return sum, sub
}
// 调用
var (
  num1 int
  num2 int
)

// 在函数中已经定义了返回值的类型
sum, sub = calc(num1, num2)

```
