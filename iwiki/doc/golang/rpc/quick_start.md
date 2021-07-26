

# 网络编程



## Go中RPC的基本使用

### 服务端
注册`rpc`服务对象，给对象绑定方法（定义类，绑定类方法）
```
rpc.RegisterName("服务名",回调对象)
```
创建监听器
```
listener,err := net.Listen()
```
建立连接
```
conn,err:=listener.Accept()
```
将连接绑定`rpc`服务
```
rpc.ServeConn(conn)
```
### 客户端

用`rpc`连接服务器 `rpc.Dial()`
```

```
调用远程函数
```
```
