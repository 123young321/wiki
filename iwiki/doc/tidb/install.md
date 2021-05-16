
# TIDB安装与部署

## TIDB单机版部署


### 下载
```
wget http://download.pingcap.org/tidb-latest-linux-amd64.tar.gz
```
### 解压文件
```
tar xf tidb-latest-linux-amd64.tar.gz

tree -L 2 tidb-v5.0.1-linux-amd64/
tidb-v5.0.1-linux-amd64/
├── bin
│   ├── arbiter
│   ├── binlogctl
│   ├── drainer
│   ├── etcdctl
│   ├── pd-ctl
│   ├── pd-recover
│   ├── pd-server
│   ├── pump
│   ├── reparo
│   ├── tidb-ctl
│   ├── tidb-server
│   ├── tikv-ctl
│   └── tikv-server
├── PingCAP\ Community\ Software\ Agreement(Chinese\ Version).pdf
└── PingCAP\ Community\ Software\ Agreement(English\ Version).pdf
```
### 启动

#### 启动PD
```
./bin/pd-server --data-dir=pd --log-file=pd.log &
```
#### 启动TIKV
```
./bin/tikv-server --pd="127.0.0.1:2379" --data-dir=tikv --log-file=tikv.log &
```
#### 启动TIDV-SERVER
```
./bin/tidb-server --store=tikv --path="127.0.0.1:2379" --log-file=tidb.log &
```
### 登录
```

```

查看服务端口
```
netstat -lntp
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:2379          0.0.0.0:*               LISTEN      5134/./bin/pd-serve
tcp        0      0 172.17.150.44:6379      0.0.0.0:*               LISTEN      3114/redis-server 1
tcp        0      0 127.0.0.1:45420         0.0.0.0:*               LISTEN      5134/./bin/pd-serve
tcp        0      0 127.0.0.1:2380          0.0.0.0:*               LISTEN      5134/./bin/pd-serve
tcp        0      0 127.0.0.1:40246         0.0.0.0:*               LISTEN      5134/./bin/pd-serve
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      2055/sshd  

```


https://www.cnblogs.com/mowei/p/7257787.html?utm_source=itdadao&utm_medium=referral

https://blog.csdn.net/weixin_39077573/article/details/95491969
