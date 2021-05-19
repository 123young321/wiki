

# 环境搭建
## 下载 Beego 和 Bee 的开发工具

```golang
go get -u github.com/astaxie/beego
go get -u github.com/beego/bee
```
安装报以下错误
```
go get: module github.com/astaxie/beego: Get "https://proxy.golang.org/github.com/astaxie/beego/@v/list": dial tcp 216.58.200.49:443: i/o timeout
```
解决：
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
# 长久添加
echo "export GOPROXY=https://goproxy.io,direct" >> ~/.profile && source ~/.profile
```
参考地址：
+ https://www.cnblogs.com/wonz/p/14038244.html
+ https://shockerli.net/post/go-get-golang-org-x-solution/
+ https://goproxy.io/zh/docs/getting-started.html 推荐阅读
+ https://github.com/goproxyio/goproxy
+ https://studygolang.com/articles/32328
+ https://github.com/goproxy/goproxy.cn/issues/93
+ https://www.cnblogs.com/zhangmingcheng/p/12294156.html
+ https://beego.me/docs/install/env.md 推荐阅读

安装完成之后，`bee` 可执行文件默认存放在 `$GOPATH/bin` 也就是 `GOBIN` 添加到环境变量中，如果没有添加可以使用以下命令：

```
echo 'export PATH="$GOPATH/bin:$PATH"' >> ~/.bashrc
source .bashrc
```
## 创建项目

`new` 命令是新建一个 Web 项目，我们在命令行下执行 `bee new <项目名>` 就可以创建一个新的项目。但是注意该命令必须在 `$GOPATH/src` 下执行。最后会在 `$GOPATH/src` 相应目录下生成如下目录结构的项目：

```
$ cd $GOPATH/src && bee new quickstart
2021/05/18 13:56:20 INFO     ▶ 0001 generate new project support go modules.
2021/05/18 13:56:20 INFO     ▶ 0002 Creating application...
        create   /opt/Golangprojects/src/quickstart/go.mod
        create   /opt/Golangprojects/src/quickstart/
        create   /opt/Golangprojects/src/quickstart/conf/
        create   /opt/Golangprojects/src/quickstart/controllers/
        create   /opt/Golangprojects/src/quickstart/models/
        create   /opt/Golangprojects/src/quickstart/routers/
        create   /opt/Golangprojects/src/quickstart/tests/
        create   /opt/Golangprojects/src/quickstart/static/
        create   /opt/Golangprojects/src/quickstart/static/js/
        create   /opt/Golangprojects/src/quickstart/static/css/
        create   /opt/Golangprojects/src/quickstart/static/img/
        create   /opt/Golangprojects/src/quickstart/views/
        create   /opt/Golangprojects/src/quickstart/conf/app.conf
        create   /opt/Golangprojects/src/quickstart/controllers/default.go
        create   /opt/Golangprojects/src/quickstart/views/index.tpl
        create   /opt/Golangprojects/src/quickstart/routers/router.go
        create   /opt/Golangprojects/src/quickstart/tests/default_test.go
        create   /opt/Golangprojects/src/quickstart/main.go
2021/05/18 13:56:20 SUCCESS  ▶ 0003 New application successfully created!
```

## 项目结构

```
$ tree -L 2 quickstart/
quickstart/
├── conf
│   └── app.conf
├── controllers
│   └── default.go
├── go.mod
├── main.go
├── models
├── routers
│   └── router.go
├── static
│   ├── css
│   ├── img
│   └── js
├── tests
│   └── default_test.go
└── views
    └── index.tpl

10 directories, 7 files

```
