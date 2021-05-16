# Docker


## Docker介绍



## Docker的基本操作

### Docker的安装

官方文档：https://docs.docker.com/engine/install/centos/#prerequisites

> 1 下载Docker依赖环境

```
yum -y install yum-utils device-mapper-persistent-data lvm2
```

> 2 指定Docker镜像源
```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
> 3 安装Docker
```
yum makecache fast
yum -y install docker-ce
```
> 4 启动Docker并测试
```
#启动docker服务
systemctl start docker
#设置开机自动启动
systemctl enable docker
#测试
docker --help
docker --version
```

### Docker的中央仓库

1. Docker官方的中央仓库(https://hub.docker.com/)
2. 国内的镜像网站:
+ 网易云:https://c.163yun.com/hub#/home
+ daoCloud:http://hub.daocloud.io/ （推荐使用）
3. 私服方式拉取镜像(添加配置)
```
touch /etc/docker/daemon.json

{
	"registry-mirrors":["https://registry.docker-cn.com"],
	"insecure-registries":["ip:port"]
}

# 重启服务
systemctl daemon-reload
systemctl restart docker
```

### 镜像的操作

> 1. 拉取镜像
```
#
docker pull nginx

```
> 2. 查看本地全部镜像
```


```
> 3. 删除本地镜像
```

```
