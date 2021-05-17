
# Docker安装与启动

官方文档：https://docs.docker.com/engine/install/centos/#prerequisites


## 下载Docker依赖环境

### 更新yum源
```
yum update
```

### 安装Docekr所需依赖，yum-utils提供yum-config-manager功能，另外两个是devicemapper驱动依赖

```
yum -y install yum-utils device-mapper-persistent-data lvm2
```

### 设置yum源为阿里源

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 安装Docker
```
yum makecache fast
yum -y install docker-ce
```

### 启动Docker

```
systemctl start docker
```

### 查看Docker版本

```
docker --help
docker --version
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
	registry-mirrors":["https://docker.mirrors.ustc.edu.cn"]
}

```

重新启动Docker
```
systemctl daemon-reload
systemctl restart docker
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
### 查看Docker状态
```
systemctl status docker
```
### 开机自启动
```
systemctl enable docker
```
### 查看Docker信息
```
docker info
```
