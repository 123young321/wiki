

# Linux安装MySQL(单实例)


## 目录规划

### 下载目录
```
mkdir /data/soft -p
```
### 数据目录
```
mkdir /data/mysql_cluster -p
```
### 安装目录
```
mkdir /opt/mysql_cluster -p
```

## 卸载Mariadb

```python
rpm -qa | grep mariadb*
yum remove mariadb*
```


## 下载与安装

官网地址：https://downloads.mysql.com/archives/community/

### 下载

```python
wget -O /data/soft/mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
```

### 解压

```python
tar zxvf /data/soft/mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz -C /opt/mysql_cluster

```

### 创建软连接

```
ln -s /opt/mysql_cluster/mysql-5.7.26-linux-glibc2.12-x86_64/ /opt/mysql_cluster/mysql
```

### 创建用户

```python
useradd -s /sbin/nologin mysql
```

### 设置环境变量

```python
vim /etc/profile
export PATH=/opt/mysql_cluster/mysql/bin:$PATH
source /etc/profile
```

### 查看版本号

```python
mysql -V
mysql  Ver 14.14 Distrib 5.7.26, for linux-glibc2.12 (x86_64) using  EditLine wrapper
```

### 创建数据目录

```python
mkdir /data/mysql_cluster/mysql_3306 -p
```

#### 挂载磁盘

```python
fdisk -l
mkfs.xfs /dev/sdb # 格式化磁盘
blkid
/dev/sdb: UUID="6e180e7c-e491-48f5-88b0-30706707bb10" TYPE="xfs"
vim /etc/fstab
UUID="6e180e7c-e491-48f5-88b0-30706707bb10" /data   xfs  defaults 0 0
mount -a
df -h
```

#### 授权

```python
 chown -R mysql.mysql /opt/mysql_cluster/*
 chown -R mysql.mysql /data/mysql_cluster/
```

### 初始化数据

[mysqld --initialize-insecure相关问题请阅读该文档](https://stackoverflow.com/questions/49205186/mysql-5-7-how-to-choose-root-password-after-mysqld-initialize)

```python
mkdir /data/mysql_cluster/mysql_3306/ -p
chown -R mysql.mysql /data/mysql_cluster/*

yum install -y libaio-devel
mysqld --initialize-insecure --user=mysql --basedir=/opt/mysql_cluster/mysql --datadir=/data/mysql_cluster/mysql_3306

mysqld --initialize-insecure --user=mysql --basedir=/opt/mysql_cluster/mysql --datadir=/data/mysql_cluster/mysql_3306
>>> 输出以下表示失败 需要安装插件
2020-10-27T14:53:41.012246Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2020-10-27T14:53:42.470450Z 0 [Warning] InnoDB: New log files created, LSN=45790
2020-10-27T14:53:42.788504Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2020-10-27T14:53:42.894731Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 34f124d6-1864-11eb-8b6c-000c29d8b98e.
2020-10-27T14:53:42.895969Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2020-10-27T14:53:42.903173Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
# 安装插件重新执行
yum install -y libaio-devel
mysqld --initialize-insecure --user=mysql --basedir=/opt/mysql_cluster/mysql --datadir=/data/mysql_cluster/mysql_3306

2021-05-17T05:50:07.840104Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2021-05-17T05:50:07.843312Z 0 [ERROR] --initialize specified but the data directory has files in it. Aborting.
2021-05-17T05:50:07.843374Z 0 [ERROR] Aborting
```

如果遇到以下错误：
```JavaScript
如果遇到以下报错
/opt/mysql_cluster/mysql/bin/mysqld: error while loading shared libraries: libnuma.so.1: cannot open shared object file: No such file or directory
```

解决：
```
1.如果已经安装了libnuma.so.1，先卸载
yum remove libnuma.so.1
2.yum -y install numactl.x86_64
```

## 编写配置文件

```python
cat >/etc/my.cnf <<EOF
[mysqld]
user=mysql
basedir=/opt/mysql_cluster/mysql
datadir=/data/mysql_cluster/mysql_3306
socket=/tmp/mysql.sock
server_id=6
port=3306
[mysql]
socket=/tmp/mysql.sock
EOF
```

## 启动数据库

### init.d 启动

将文件mysql启动文件脚本复制到 `/etc/init.d/mysqld`

```python
cp /opt/mysql_cluster/mysql/support-files/mysql.server  /etc/init.d/mysqld
```

启动 mysql

```
service mysqld restart
/etc/init.d/mysqld restart
```

### systemd 启动


```python
cat >/etc/systemd/system/mysqld.service <<EOF
[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target
[Install]
WantedBy=multi-user.target
[Service]
User=mysql
Group=mysql
ExecStart=/application/mysql/bin/mysqld --defaults-file=/etc/my.cnf
LimitNOFILE = 5000
EOF
```

## 管理员密码

### 设置管理员密码

```python
mysqladmin -uroot -p password 123456
```

### 重置管理员密码

```python

```


# Linux安装MySQL(多实例)
