# 常用的应用部署

## MySQL部署

### 拉取mysql镜像

镜像地址：https://hub.docker.com/r/centos/mysql-57-centos7

```python
docker pull centos/mysql-57-centos7
```
#### 创建容器
```
docker run -di --name=mysql57_3306 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=lipanpan@3 centos/mysql-57-centos7
```
参数介绍：
+ -p 端口映射，格式为：`宿主机映射端口:容器运行端口`
+ -e 环境变量，代表添加环境变量： `MYSQL_ROOT_PASSWORD` 是root用户登录密码

## 部署Tomcat

### 拉取镜像
```
docker pull tomcat:7-jre7/
```
### 创建容器
```

```
参数介绍：

+ -p 端口映射

```

```


## 部署Nginx
参数地址：https://www.cnblogs.com/panchanggui/p/12037974.html
### 拉取镜像
```
docker pull nginx
```
### 创建容器
```
docker -di --name=nginx_80 -p 80:80 nginx
```
## Redis部署

### 拉取镜像
```python
docker pull redis
```
### 创建容器
```
docker run -itd --name redis_6379 -p 6379:6379 redis:latest
```
### 连接Redis的方式

#### 登录容器

```
docker exec -it redis_6379 /bin/bash
redis-cli
```

#### 远程连接
```
docker exec -it redis_6379 redis-cli -h 192.168.132.201 -p 6379
```

### 查看日志
```
docker logs redis_6379
```

### 自定义配置文件创建

#### 配置文件
```
### 以守护进行模式启动
daemonize no
### 绑定主机ip地址 Your IP
# bind 192.168.120.110
### 监听端口
port 6379
### pid文件存放位置
# pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
### log文件存放位置没有权限
# logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log
### 设置数据库的数量
databases 16
### 指定本地持久化文件的文件名,默认为dump.rdb
dbfilename redis_6379.rdb
### 本地数据库目录
# dir /data/redis_cluster/redis_6379
### 配置密码
requirepass redis@6379@3
```
#### 创建数据目录
```
mkdir /data/docker/data/redis_cluster/redis_6379/{conf,logs,data} -p
```

### 创建容器
```
docker container run -idt --name redis_6379 -p6379:6379 -v `pwd`/conf:/usr/local/etc/redis -v `pwd`/data:/data redis:latest redis-server /usr/local/etc/redis/redis_6379.conf
```
### 连接Redis

#### 登录容器
```
docker exec -it redis_6379 /bin/bash
redis-cli -a redis@6379@3
```
#### 远程连接
```
docker exec -it redis_6379 redis-cli -h 192.168.132.201 -p 6379 -a redis@6379@3
```


参考教程：

+ https://www.runoob.com/docker/docker-tutorial.html
+ https://blog.csdn.net/qq_40794973/article/details/91484107
+ https://www.cnblogs.com/brady-wang/p/12366287.html
+ https://www.jianshu.com/p/4413f484789d
https://www.cnblogs.com/zhzhlong/p/9465670.html
https://blog.csdn.net/qq_43599835/article/details/103019447

https://www.cnblogs.com/hg-super-man/p/10908216.html
https://www.modb.pro/db/45946

部署yum源安装ansible
https://www.cnblogs.com/shenyuanhaojie/p/14138158.html



https://blog.csdn.net/ksj367043706/article/details/88779121

https://www.cnblogs.com/jesse131/p/13543308.html

https://blog.52itstyle.vip/archives/2402/
