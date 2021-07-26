
# gRPC
gRPC是一个高性能、开源和通用的RPC框架，面向移动和HTTP/2设计。gRPC基于HTTP/2标准设计，带来诸如双向流、流控、头部压缩、单TCP连接的多复用请求等。这些特性使得其在移动设备上表现更好，更省电和节省空间占用


## RPC

RPC(Remote Procedure Call Protocol) 远程过程调用协议，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。简单来说，就是根远程访问或者web请求差不多，都是一个client向远端服务器请求返回结果，但是web请求使用的是网络协议是http高层协议，而rpc所使用的协议多为TCP，是网络层协议，减少了信息的包装，加快了处理速度。golang本身有rpc包，可以方便的使用，来构建自己的rpc服务，下边是一个简单是实例，可以加深我们的理解。

### 调用流程
1. 调用客户端句柄；执行传送参数
2. 调用本地系统内核发送网络消息
3. 消息传送到远程主机
4. 服务器句柄得到消息并取得参数
5. 执行远程过程
6. 执行的过程将结果返回服务器句柄
7. 服务器句柄返回结果，调用远程系统内核
8. 取消传回本地主机
9. 客户句柄由内核接收消息
10. 客户接收句柄返回的数据


示例：基本的web服务代码
```
package main

import (
	"fmt"
	"io"
	"net"
	"net/http"
)

func pandatext(w http.ResponseWriter, r *http.Request) {
	io.WriteString(w, "hello word hello panda")
}

func main() {
	// 页面请求
	http.HandleFunc("/panda", pandatext)
	// 监听端口
	ln, err := net.Listen("tcp", ":10086")
	if err != nil {
		fmt.Println("网络错误")
	}
	// 启动服务
	http.Serve(ln, nil)
}


```
示例：golang实现rpc


```
package main

import (
	"fmt"
	"io"
	"net"
	"net/http"
	"net/rpc"
)

/**
基于Golang实现RPC的必须条件
1. 方法是导出的
2. 方法有两个参数
3. 方法的第二个参数是指针
4. 方法只有一个error接口类型的返回值
*/

type Panda int

// 函数关键字(对象) 函数名 (对端发送过来的内容,返回给对端的内容) 错误返回值
func (p *Panda) GetInfo(argType int, replyType *int) error {
	fmt.Println("打印对端发送过来的内容：，", argType)
	// 修改内容
	*replyType = argType + 12306
	return nil
}

func pandatextV2(w http.ResponseWriter, r *http.Request) {
	io.WriteString(w, "hello word hello panda")
}

func main() {
	// 页面请求
	http.HandleFunc("/pandaV2", pandatextV2)
	// 将类实例化为对象
	pd := new(Panda)
	rpc.Register(pd)
	rpc.HandleHTTP()
	// 监听端口
	ln, err := net.Listen("tcp", ":10087")
	if err != nil {
		fmt.Println("网络错误")
	}
	// 启动服务
	http.Serve(ln, nil)
}


```

client.go
```
package main

import (
	"fmt"
	"net/rpc"
)

func main() {
	// 建立网络连接
	cli, err := rpc.DialHTTP("tcp", "127.0.0.1:10087")
	if err != nil {
		fmt.Println("网络连接失败")
	}
	var pd int
	err = cli.Call("Panda.GetInfo", 10086, &pd)
	fmt.Println(err)
	if err != nil {
		fmt.Println("调用失败")
	}
	fmt.Println("最后得到的值：", pd)
}

```

### 示例GRPC的基本案例
