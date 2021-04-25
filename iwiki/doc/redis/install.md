# 安装

## 目录结构规划

+ 下载目录
+ 数据目录
+ 安装目录

### 下载目录
```
mkdir /data/soft -p
```
### 数据目录
```
mkdir -p /data/redis_cluster/redis_6379
```
### 安装目录
```
mkdir -p /opt/redis_cluster/redis_6379/{conf,pid,logs}
```
安装目录结构
```
tree /opt/
/opt/
└── redis_cluster
    └── redis_6379
        ├── conf
        ├── logs
        └── pid
```

## 下载
```
wget -O /data/soft/redis-3.2.9.tar.gz http://download.redis.io/releases/redis-3.2.9.tar.gz
```

## 编译安装

+ 解压
+ 创建软连接
+ 安装

### 解压
```
tar zxvf redis-3.2.9.tar.gz -C /opt/redis_cluster/
```
### 创建软连接
```
ln -s /opt/redis_cluster/redis-3.2.9/ /opt/redis_cluster/redis
```
### 编译
```
cd /opt/redis_cluster/redis
make && make install
```

## 配置
```
vim /opt/redis_cluster/redis_6379/conf/redis_6379.conf
### 以守护进行模式启动
daemonize yes
### 绑定主机ip地址
bind 192.168.120.110
### 监听端口
port 6379
### pid文件存放位置
pidfile /opt/redis_cluster/redis_6379/pid/redis_6379.pid
### log文件存放位置
logfile /opt/redis_cluster/redis_6379/logs/redis_6379.log
### 设置数据库的数量
databases 16
### 指定本地持久化文件的文件名,默认为dump.rdb
dbfilename redis_6379.rdb
### 本地数据库目录
dir /data/redis_cluster/redis_6379
```
> [!TIP|style:flat|label:注意|iconVisibility:visible] 
> Redis默认提供了安装脚本可以在此基础进行更改
> vim /opt/redis_cluster/redis-3.2.9/utils/install_server.sh 

## 启动Redis
```
redis-server /opt/redis_cluster/redis_6379/conf/redis_6379.conf
```

### 查看进程
```
ps -ef | grep redis
root        657      1  0 12:34 ?        00:00:04 /usr/local/bin/redis-server 127.0.0.1:6379
root       4451      1  0 13:32 ?        00:00:00 redis-server 192.168.120.110:6379
root       4743   1139  0 13:33 pts/0    00:00:00 grep --color=auto redis
```
### 启动redis客户端
```
redis-cli -h 192.168.120.110
```


