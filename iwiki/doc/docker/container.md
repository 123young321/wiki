# Docker容器

## 常用命令

### 查看容器
查看当前正在运行的容器
```python
docker ps
```
容器的详细展示列项
```
$ docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED         STATUS         PORTS     NAMES
504052448fe8   centos:latest   "/bin/bash"   4 minutes ago   Up 4 minutes             centos_version_7
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

常用的占位符

| 名称        | 含义                 |
| ----------- | -------------------- |
| .ID         | 容器ID               |
| .Image      | 镜像ID               |
| .Command    | 执行的命令           |
| .CreatedAt  | 容器创建时间         |
| .RunningFor | 运行时长             |
| .Ports      | 暴露的端口           |
| .Status     | 容器状态             |
| .Names      | 容器名称             |
| .Label      | 分配给容器的所有标签 |
| .Mounts     | 容器挂载的卷         |
| .Networks   | 容器所用的网络名称   |


参考地址：

+ https://www.runoob.com/docker/docker-ps-command.html


### 创建启动容器

官网地址：https://docs.docker.com/engine/reference/run/

```
docker run
```

常用参数介绍

| 名称   | 含义                                                         |
| ------ | ------------------------------------------------------------ |
| -i     | 运行容器                                                     |
| -t     | 表示容器启动后会进入其命令行，加入这两个参数后，容器创建就能登录进去，即分配一个伪终端 |
| --name | 为创建的容器命名                                             |
| -v     | 表示目录映射关系（前者是宿主机目录，后者是映射到宿主机上的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上 |
| -d     | 在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器） |
| -p     | 表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射 |
| -ip    | 为容器制定一个固定的ip                                       |
| --net  | 指定网络模式                                                 |
| --rm   | (不能与-d同时使用)                                           |

#### 创建容器的方式

#### 交互式创建

示例：创建名称为 test_centos 的容器
```
$ docker run -it --name="test_centos" centos:latest /bin/bash
[root@504052448fe8 /]#   
```

示例：创建容器后查看容器状态
```python
$ docker ps
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS          PORTS     NAMES
609df356e477   centos:latest   "/bin/bash"   24 seconds ago   Up 23 seconds             test_centos
```
退出容器
```
exit
```
示例：退出容器后查看容器状态
````
$ docker ps -l # -l 查看停止的容器
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS                      PORTS     NAMES
609df356e477   centos:latest   "/bin/bash"   14 minutes ago   Exited (0) 11 seconds ago             test_centos
```

#### 守护式创建

### 停止与启动容器

#### 停止容器
```python
docker stop 容器名称(或者容器ID)
```
#### 启动容器
```python
docker start 容器名称(或者容器ID)
```
