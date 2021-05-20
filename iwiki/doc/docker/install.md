
# Docker安装与启动

官方文档：https://docs.docker.com/engine/install/centos/#prerequisites


## 下载安装

### 卸载旧版本
```
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine

```

### 更新yum源
```
yum update -y
```

### 安装依赖

yum-utils提供yum-config-manager功能，另外两个是devicemapper驱动依赖

```python
yum -y install yum-utils device-mapper-persistent-data lvm2
```

### 设置yum源为阿里源

```python
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 安装
```python
yum makecache fast && yum -y install docker-ce
```

### 启动

```python
systemctl start docker
```

### 查看版本

```python
docker --help && docker --version
```

### 设置ustc的镜像
ustc是老牌的Linux镜像服务提供者，使用ustc的docker加速器速度很快。ustc docker mirrors的优势是不需要注册

编辑 `/etc/docker/daemon.json` 文件

```python
vim /etc/docker/daemon.json
```

添加如下内容

```
{
	"registry-mirrors":["https://docker.mirrors.ustc.edu.cn"]
}

```

重新启动

```
systemctl daemon-reload && systemctl restart docker
```


## Docker启动与停止

### 启动

```
systemctl start docker
```

### 停止

```
systemctl stop docker
```

### 状态

```
systemctl status docker
```

### 开机自启动

```
systemctl enable docker
```

### 基本信息

```
docker info
```
