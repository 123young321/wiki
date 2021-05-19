# Docker容器

## 常用命令

### 查看容器
查看当前正在运行的容器
```python
docker ps
```
查看所有的容器
```
docker ps -a
```
查看最后一次运行的容器
```
docker ps -l
```
查看停止的容器
```
docker ps -f status=exited
```
显示指定的列 `docker ps` 默认的显示内容过多，当值过长时就会导致折行，可读性很差，所以希望只显示自己关心的某些列

示例：显示 `CONTAINER` `ID` `PORTS`

占位符说明：

+ `table`：表示显示表头列名
+ `{{ID}}`：容器ID
+ `{{Command}}`：启动执行命令

```
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Ports}}"
```

可用的占位符



参考地址：
+ https://www.runoob.com/docker/docker-ps-command.html


### 创建启动容器

官网地址：https://docs.docker.com/engine/reference/run/

```
docker run
```

常用参数介绍
