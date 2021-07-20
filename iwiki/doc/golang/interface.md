# 接口

## 定义接口
```
type 接口名称 interface {
  方法名 返回值
}
```
示例：
```
type Base interface {
	f1()                   // 定义方法
	f2() int               // 定义方法 返回int类型
	f3() (int bool)        // 定义方法 2个返回值 int bool类型
	f4(n1 int, n2 int) int // 定义方法 需要两个参数 一个返回值
}

type empty interface{} // interface
```
接口的方法只是定义，不能实现具体的逻辑
## 接口的作用
在程序中接口一般有两大作用代指类型和约束

### 空接口
空接口代指任意类型
```


```
