# 常用的应用部署

## MySQL部署

### 拉取mysql镜像
```python
docker pull centos/mysql-57-centos7
```
### 创建挂载目录
+ 数据目录
+ 日志
+ 配置文件

```

```

### 创建容器
```
docker run -di --name=mysql57_3306_test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

参数介绍：
+ -p 端口映射，格式为：`宿主机映射端口:容器运行端口`
+ -e 环境变量，代表添加环境变量： `MYSQL_ROOT_PASSWORD` 是root用户登录密码

### 进入MySQL容器
```
docker exec -it mysql57_3306 /bin/bash
```
### 登录MySQL
```
mysql -uroot -p
```

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
```
docker pull redis
```
### 创建容器
```
docker -di --name=redis_6379 -p 6379:6379 redis
```



##


参考教程：

+ https://www.runoob.com/docker/docker-tutorial.html
+ https://blog.csdn.net/qq_40794973/article/details/91484107
