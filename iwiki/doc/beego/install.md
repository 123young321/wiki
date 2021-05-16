

# 环境搭建
## 下载 Beego 和 Bee 的开发工具

```golang
go get -u github.com/astaxie/beego
go get -u github.com/beego/bee

```
安装完成之后，`bee` 可执行文件默认存放在 `$GOPATH/bin` 也就是 `GOBIN` 添加到环境变量中，如果没有添加可以使用以下命令：
```
echo 'export PATH="$GOPATH/bin:$PATH"' >> ~/.bashrc
source .bashrc
```
## 创建项目

```
bee new quickstart
```
> 通过 bee 创建的项目默认的位置会在 $PATH/src

## 目录结构
```

```
