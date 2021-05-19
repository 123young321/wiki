
# Docker镜像



## 常用命令
### 查看镜像
查看本地当前有哪些镜像，可以使用该命令：

```python
docker images
```

#### 镜像的基本信息

```python
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/redis     latest              bc8d70f9ef6c        4 days ago          105 MB
```

+ REPOSITORY：镜像名称
+ TAG：镜像标签
+ IMAGE ID：镜像ID
+ CREATED：镜像的创建日期
+ SIZE：镜像大小

镜像默认的存储位置：`var/lib/docker`

#### 常用参数

```python
$ docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options:
  -a, --all             Show all images (default hides intermediate images)
      --digests         Show digests
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print images using a Go template
      --help            Print usage
      --no-trunc        Don't truncate output
  -q, --quiet           Only show numeric IDs

```

### 搜索镜像

网络中搜索需要的镜像，可以使用该命令：

```python
docker search 镜像名称
```

#### 搜索基本列信息

+ INDEX：仓库名称       
+ NAME：镜像名称                                 
+ DESCRIPTION：描述                                     
+ STARS：用户评价
+ OFFICIAL：是否是官方   
+ AUTOMATED：自动构建，表示该镜像由Docker Hub自动构建流程构建的

### 拉取镜像
拉取镜像表示从中央仓库拉取镜像

```python
docker pull 镜像名称
```

示例：下载Redis的镜像

```
$ docker pull docker.io/redis
Using default tag: latest
Trying to pull repository docker.io/library/redis ...
latest: Pulling from docker.io/library/redis
69692152171a: Pull complete
a4a46f2fd7e0: Pull complete
bcdf6fddc3bd: Pull complete
b7e9b50900cc: Pull complete
5f3030c50d85: Pull complete
63dae8e0776c: Pull complete
Digest: sha256:365eddf64356169aa0cbfbeaf928eb80762de3cc364402e7653532bcec912973
Status: Downloaded newer image for docker.io/redis:latest
```

### 删除镜像

基于镜像ID删除镜像
```python
docker rmi 镜像ID
```

删除所有镜像
```
docker rmi `docker images -q`
```
