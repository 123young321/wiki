

# Linux安装MySQL


## 目录规划

下载目录
```
mkdir /data/soft -p
```
数据目录
```
mkdir /data/mysql
```
安装目录
```
mkdir /opt/mysql/
```

## 卸载Mariadb

```python
rpm -qa | grep mariadb*
yum remove mariadb*
```


## 下载与安装
官网地址：

### 下载

```python
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.26-linux-glibc2.12-x86_64.tar
```

### 解压

```python
tar xf mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
mv mysql-5.7.26-linux-glibc2.12-x86_64  /application/mysql

```

### 创建用户

```python
useradd -s /sbin/nologin mysql
```

### 设置环境变量

```python
vim /etc/profile
export PATH=/application/mysql/bin:$PATH
source /etc/profile
mysql -V
mysql  Ver 14.14 Distrib 5.7.26, for linux-glibc2.12 (x86_64) using  EditLine wrapper
```

### 创建数据目录

```python
mkdir /data
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
 chown -R mysql.mysql /application/*
 chown -R mysql.mysql /data
```

#### 初始化数据

创建系统数据
```python
mkdir /data/mysql/data -p
chown -R mysql.mysql /data
yum install -y libaio-devel
mysqld --initialize-insecure --user=mysql --basedir=/application/mysql --datadir=/data/mysql/data

[root@db-201 ~]# mysqld --initialize-insecure --user=mysql --basedir=/application/mysql --datadir=/data/mysql/data
2020-10-27T14:53:41.012246Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2020-10-27T14:53:42.470450Z 0 [Warning] InnoDB: New log files created, LSN=45790
2020-10-27T14:53:42.788504Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2020-10-27T14:53:42.894731Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 34f124d6-1864-11eb-8b6c-000c29d8b98e.
2020-10-27T14:53:42.895969Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2020-10-27T14:53:42.903173Z 1 [Warning] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.

```

## 编写配置文件

```python
cat >/etc/my.cnf <<EOF
[mysqld]
user=mysql
basedir=/application/mysql
datadir=/data/mysql/data
socket=/tmp/mysql.sock
server_id=6
port=3306
[mysql]
socket=/tmp/mysql.sock
EOF
```

## 启动数据库
> sys-v 启动方式

```python
1. 将文件复制到/etc/init.d/mysqld
cp /application/mysql/support-files/mysql.server  /etc/init.d/mysqld
service mysqld restart
/etc/init.d/mysqld restart
```
> systemctl 启动方式

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
11. 设置管理员密码
```python
mysqladmin -uroot -p password 123456
```
### 1.1 重置管理员密码


### 1.2 用户权限管理

#### 1.2.1 用户管理

用户的作用
+ 登陆MySQL
+ 管理MySQL

用户的定义
```sql
用户名@'白名单’
举例说明：
wordpress@'%'  -- 任何地址都可以访问 不安装
wordpress@'localhost'
wordpress@'127.0.0.1'
wordpress@'10.0.0.%'
wordpress@'10.0.0.5%'
wordpress@'10.0.0.0/255.255.254.0'
wordpress@'10.0.%'
```
用户的操作

查询当前数据库的用户信息 `mysql.user`
```
mysql> select user,host from mysql.user;
+---------------+-----------+
| user          | host      |
+---------------+-----------+
| mysql.session | localhost |
| mysql.sys     | localhost |
| root          | localhost |
+---------------+-----------+
3 rows in set (0.00 sec)

```
创建用户并设置密码
```sql
mysql> create user dev@'192.168.%' identified by '123';
Query OK, 0 rows affected (0.00 sec)
# 8.0 版本支持该语句，创建用户并同时授权
grant all on *.* to dev@'10.0.0.%' identified by '123' with grant option;

```
修改用户密码
```sql
alter user dev@'192.168.%' identified by '123456';
```
删除用户
```sql
drop user dev@'192.168.%';
```

 #### 1.2.2 权限管理
##### 权限列表
```
ALL : 全部的权限
SELECT,INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, SHUTDOWN, PROCESS, FILE, REFERENCES, INDEX, ALTER, SHOW DATABASES, SUPER, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER, CREATE TABLESPACE

with grant option ：可以为其他人进行授权
```
##### 授权命令
###### 查询用户的权限
```
mysql> show grants for root@localhost;
+---------------------------------------------------------------------+
| Grants for root@localhost                                           |
+---------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION |
| GRANT PROXY ON ''@'' TO 'root'@'localhost' WITH GRANT OPTION        |
+---------------------------------------------------------------------+
2 rows in set (0.00 sec)

```
###### 给用户授权
```
grant all on *.* to dev@'192.168.%' identified by '123' with grant option;

命令的解释说明：
grant 权限 on 作用目标 to 用户 identified by 密码 with grant option;


作用目标
  *.* : mysql下所有的库和表全部授权
  wordpress.* : wordpress下的所有表全部授权
  wordpress.t1 : wordpress下的t1表授权

案例
  创建一个管理员用户root 可以通过10网段 管理数据库
  	grant all on *.* to root@'10.0.0.%' identified by '123' with grant option;
  创建一个应用用户 wordpress 可以通过10网段 wordpress库所有的表进行
	grant SELECT,INSERT,UPDATE,DELETE on wordpress.* to wordpress@'10.0.0.%' identified by '123';

```
###### 查看用户的权限
```sql
mysql> show grants for dev@'192.168.%';
+--------------------------------------------------------------------+
| Grants for dev@192.168.%                                           |
+--------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'dev'@'192.168.%' WITH GRANT OPTION |
+--------------------------------------------------------------------+
1 row in set (0.00 sec)

```
###### 回收权限
```sql
revoke delete on wordpress.*  from wordpress@'10.0.0.%';
```

### 1.3 MySQL的连接管理
#### 1.3.1 mysql命令
##### 连接方式
###### TCPIP
```
# 需求从某个网段登陆到数据库
grant all on *.* to root@'39.105.100.%' identified by '123456';

mysql -uroot -p -h 39.105.100.168 -P3306

```
###### Socket
```
mysql -uroot -p -S /tmp/mysql.sock
```

#### 1.3.2 mysql内置的功能

##### 1. 连接数据库
```
-u
-p
-S
-h
-p
-e 免交互
< 恢复数据
举例说明

mysql -uroot -p -S /tmp/mysql.sock
# 不需要连接到mysql终端就可以获取到相应的结果    
 [root@db-201 etc]# mysql -uroot -p -e "show databases;"
Enter password:
+--------------------+
| Database           |
+--------------------+
| information_schema |
| dbtest             |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
# < 恢复数据
myql -uroot -p123 < /root<world.sql
```
###### 2 内置命令
+ help
```
For information about MySQL products and services, visit:
   http://www.mysql.com/
For developer information, including the MySQL Reference Manual, visit:
   http://dev.mysql.com/
To buy MySQL Enterprise support, training, or other products, visit:
   https://shop.mysql.com/

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
resetconnection(\x) Clean session context.

For server side help, type 'help contents'

常用指令说明
\c   ctrl+c 打印mysql帮助
\q   quit ; exit; ctrl + d 退出myql
\G                           数据格式化输出
source                     数据恢复

```










### 1.3 初始化配置

#### 1.3.1 初始化配置方法
##### 方法介绍
1. 初始化配置文件
2. 启动命令行
3. 预编译时设置(仅限制编译安装)
##### 配置文件书写格式
```sql
[标签]
key=value
[标签]
key=value
```
##### 配置文件标签的归类
###### 服务器端标签归类
```
[mysqld]
[mysql_safe]
[server]
注意
  当使用 server 的时候包含 mysqld 和 mysql_safe
```
###### 客户端标签归类
```
[mysql]
[mysqladmin]
[mysqldump]
[client]
注意
  当使用 client 的时候 包含 mysql 和 mysqladmin 和 mysqldump
```
###### 配置文件设置样板
```
# 服务器端配置
[mysqld]
# 用户
user=mysql
# 软件安装目录
basedir=/application/mysql
# 数据路径
datadir=/data/mysql/data
# socket 文件位置
socket=/tmp/mysql.sock
# 服务器 id (主从复制 1-65535)
server_id=6
# 端口号
port=3306
# 客户端配置
[mysql]
socket=/tmp/mysql.sock
```
###### 配置文件顺序
```
mysqld --help --version |grep my.cnf
/etc/my.cnf
/etc/mysql/my.cnf
/usr/local/mysql/etc/my.cnf
~/.my.cnf   # 注意 .my.cnf
注意：
    配置文件按照从左到右读取如果有其他配置文件会被覆盖
```
###### 指定配置文件的启动
```
# 手工定制文件
-- defaults-file=/etc/my.cnf
# 当使用该种方式进行启动的时候需要
mysqld_safe --defaults-file=/tmp/aa.txt

# 例如 sysd
cat /etc/systemd/system/mysqld.service
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
```

## 2. 多实例搭建
### 2.1 创建实例数据目录
```
mkdir -p /data/330{7,8,9}/data
```
### 2.2 创建多个配置文件
```
cat > /data/3307/my.cnf <<EOF
[mysqld]
basedir=/application/mysql
datadir=/data/3307/data
socket=/data/3307/mysql.sock
log_error=/data/3307/mysql.log
port=3307
server_id=7
log_bin=/data/3307/mysql-bin
EOF

cat > /data/3308/my.cnf <<EOF
[mysqld]
basedir=/application/mysql
datadir=/data/3308/data
socket=/data/3308/mysql.sock
log_error=/data/3308/mysql.log
port=3308
server_id=8
log_bin=/data/3308/mysql-bin
EOF

cat > /data/3309/my.cnf <<EOF
[mysqld]
basedir=/application/mysql
datadir=/data/3309/data
socket=/data/3309/mysql.sock
log_error=/data/3309/mysql.log
port=3309
server_id=9
log_bin=/data/3309/mysql-bin
EOF

```
初始化数据
```
mv /etc/my.cnf /etc/my.cnf.bak
mysqld --initialize-insecure  --user=mysql --datadir=/data/3307/data --basedir=/application/mysql
mysqld --initialize-insecure  --user=mysql --datadir=/data/3308/data --basedir=/application/mysql
mysqld --initialize-insecure  --user=mysql --datadir=/data/3309/data --basedir=/application/mysql
```
systemd管理多实例
```
cd /etc/systemd/system
cp mysqld.service mysqld3307.service
cp mysqld.service mysqld3308.service
cp mysqld.service mysqld3309.service

vim mysqld3307.service
# 修改为:
ExecStart=/application/mysql/bin/mysqld  --defaults-file=/data/3307/my.cnf
vim mysqld3308.service
# 修改为:
ExecStart=/application/mysql/bin/mysqld  --defaults-file=/data/3308/my.cnf
vim mysqld3309.service
# 修改为:
ExecStart=/application/mysql/bin/mysqld  --defaults-file=/data/3309/my.cnf

grep "ExecStart" mysqld3309.service
ExecStart=/application/mysql/bin/mysqld --defaults-file=/data/3309/my.cnf
grep "ExecStart" mysqld3308.service
ExecStart=/application/mysql/bin/mysqld --defaults-file=/data/3308/my.cnf
grep "ExecStart" mysqld3307.service
ExecStart=/application/mysql/bin/mysqld --defaults-file=/data/3307/my.cnf
```
授权
```
chown -R mysql.mysql /data/*
```
启动
```
systemctl start mysqld3307.service
systemctl start mysqld3308.service
systemctl start mysqld3309.service

```
验证多实例
```
netstat -lnp|grep 330
mysql -S /data/3307/mysql.sock -e "select @@server_id"
mysql -S /data/3308/mysql.sock -e "select @@server_id"
mysql -S /data/3309/mysql.sock -e "select @@server_id"

```
连接多实例
```
mysql -S /data/3307/mysql.sock
mysql -S /data/3308/mysql.sock
mysql -S /data/3309/mysql.sock
```


## 3 SQL基础应用
### 3.1 SQL介绍
结构化的查询语句，关系行数据库通用命令，遵循SQL92的标准(SQL_MODE)

### 3.2 SQL语句分类

+ DDL 数据定义语言
+ DCL 数据控制语言
+ DML 数据操作语言
+ DQL 数据查询语言


### 3.3 数据库的逻辑结构
#### 3.3.1 库


#### 3.3.2 表


## 4 主从复制
**简介**
1. 主从复制基于binlog来实现的
2. 主库发生新的操作,都会记录binlog
3. 从库取得主库的binlog进行回放
4.  主从复制的过程是异步

**前提**
1. 2个或以上的数据库实例
2. 主库需要开启二进制日志
3. server_id要不同,区分不同的节点
4. 主库需要建立专用的复制用户 (replication slave)
5. 从库应该通过备份主库,恢复的方法进行"补课"
6. 人为告诉从库一些复制信息(ip port user pass,二进制日志起点)
7. 从库应该开启专门的复制线程

### 4.1 主从复制搭建
#### 4.1.1 准备多实例
```
[root@db-201 ~]# systemctl start mysqld3307.service
[root@db-201 ~]# systemctl start mysqld3308.service
[root@db-201 ~]# systemctl start mysqld3309.service

[root@db-201 ~]# mysql -S /data/3307/mysql.sock -e "select @@server_id"
+-------------+
| @@server_id |
+-------------+
|           7 |
+-------------+
[root@db-201 ~]# mysql -S /data/3308/mysql.sock -e "select @@server_id"
+-------------+
| @@server_id |
+-------------+
|           8 |
+-------------+
[root@db-201 ~]# mysql -S /data/3309/mysql.sock -e "select @@server_id"
+-------------+
| @@server_id |
+-------------+
|           9 |
+-------------+

```
#### 4.1.2 主从配置文件介绍
+ 主库必须开启二进制文件
+ 两个节点：server_id 是否不同
```
[mysqld]
basedir=/application/mysql
datadir=/data/3307/data
socket=/data/3307/mysql.sock
log_error=/data/3307/mysql.log
port=3307
server_id=7
log_bin=/data/3307/mysql-bin
```
#### 4.1.3 主库创建复制用户
```
mysql -uroot -S /data/3307/mysql.sock -e "grant replication slave on *.* to repl@'192.168.%' identified by '123';"

grant replication slave on *.* to repl@'192.168.%' identified by '123';

mysql> select user,host from mysql.user;
+---------------+-----------+
| user          | host      |
+---------------+-----------+
| repl          | 192.168.% |
| mysql.session | localhost |
| mysql.sys     | localhost |
| root          | localhost |
+---------------+-----------+
4 rows in set (0.01 sec)


```
#### 4.1.4 主库备份
```
# 主库备份
[root@db-201 ~]# mysqldump -uroot -S /data/3307/mysql.sock -A --master-data=2 --single-transaction -R -E --triggers > /tmp/full.sql
# 从库导入数据
mysql> set sql_log_bin=0;
Query OK, 0 rows affected (0.00 sec)
mysql> source /tmp/full.sql

```
#### 4.1.5 从库同步
```
help change master to

CHANGE MASTER TO
  MASTER_HOST='192.168.17.201',
  MASTER_USER='repl',
  MASTER_PASSWORD='123',
  MASTER_PORT=3307,
  MASTER_LOG_FILE='mysql-bin.000002',
  MASTER_LOG_POS=445,
  MASTER_CONNECT_RETRY=10;


查看备份的 sql 查看备份sql 二进制文件
vim /tmp/full.sql
-- CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000002', MASTER_LOG_POS=445;


mysql> CHANGE MASTER TO
    ->   MASTER_HOST='192.168.17.201',
    ->   MASTER_USER='repl',
    ->   MASTER_PASSWORD='123',
    ->   MASTER_PORT=3307,
    ->   MASTER_LOG_FILE='mysql-bin.000002',
    ->   MASTER_LOG_POS=445,
    ->   MASTER_CONNECT_RETRY=10;
Query OK, 0 rows affected, 2 warnings (0.02 sec)
```
#### 4.1.6 开启复制线程(IO,SQL)
```
mysql> start slave;
Query OK, 0 rows affected (0.01 sec)
```
#### 4.1.7 检查主从复制状态
```
mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.17.201
                  Master_User: repl
                  Master_Port: 3307
                Connect_Retry: 10
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 445
               Relay_Log_File: db-201-relay-bin.000002
                Relay_Log_Pos: 320
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 445
              Relay_Log_Space: 528
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 7
                  Master_UUID: cfd364e2-28e9-11eb-939f-000c29d8b98e
             Master_Info_File: /data/3308/data/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:
1 row in set (0.00 sec)


[root@db-201 ~]# mysql -S /data/3308/mysql.sock -e "show slave status\G" | grep Yes
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes

```

#### 4.1.8 重新连接主从
```
stop slave
reset slave all;
change master to

mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000002 |      598 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)

```
### 4.2 主从复制监控命令的介绍

```
mysql> show slave status\G
*************************** 1. row ***************************
# 主库相关信息(master.info)
Slave_IO_State: Waiting for master to send event
Master_Host: 192.168.17.201
Master_User: repl
Master_Port: 3307
Connect_Retry: 10
——————————————————
* Master_Log_File: mysql-bin.000002
* Read_Master_Log_Pos: 598
——————————————————

# 从库relay应用信息相关(relay.info)
Relay_Log_File: db-201-relay-bin.000002
Relay_Log_Pos: 473
Relay_Master_Log_File: mysql-bin.000002

#——————————————————
[root@db-201 data]# ls |grep info
master.info
relay-log.info
#——————————————————

# 从库线程状态
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
# 从库连接状态排错
Last_IO_Errno: 0
Last_IO_Error:
Last_SQL_Errno: 0
Last_SQL_Error:

# 过滤复制相关：使用场景 有选择的同步主库中的库
Replicate_Do_DB:
Replicate_Ignore_DB:
Replicate_Do_Table:
Replicate_Ignore_Table:
Replicate_Wild_Do_Table:
Replicate_Wild_Ignore_Table:

# 从库延时的时间(秒)
Seconds_Behind_Master: 0

# 延时从库：使用场景 主库误删除数据防止从库立即删除
SQL_Delay: 0
SQL_Remaining_Delay: NULL

# GTID复制有关的状态信息
Retrieved_Gtid_Set:
Executed_Gtid_Set:
Auto_Position: 0


Last_Errno: 0
Last_Error:
Skip_Counter: 0
Exec_Master_Log_Pos: 598
Relay_Log_Space: 681
Until_Condition: None
Until_Log_File:
Until_Log_Pos: 0
Master_SSL_Allowed: No
Master_SSL_CA_File:
Master_SSL_CA_Path:
Master_SSL_Cert:
Master_SSL_Cipher:
Master_SSL_Key:

Master_SSL_Verify_Server_Cert: No
Replicate_Ignore_Server_Ids:
Master_Server_Id: 7
Master_UUID: cfd364e2-28e9-11eb-939f-000c29d8b98e
Master_Info_File: /data/3308/data/master.info

Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
Master_Retry_Count: 86400
Master_Bind:
Last_IO_Error_Timestamp:
Last_SQL_Error_Timestamp:
Master_SSL_Crl:
Master_SSL_Crlpath:

Replicate_Rewrite_DB:
Channel_Name:
Master_TLS_Version:
1 row in set (0.00 sec)

```
### 4.3 主从复制故障
#### 4.3.1 从库IO线程故障
**连接主库故障**：connecting 网络，连接信息错误或变更，防火墙，连接数上限了

**排查思路**: 使用复制用户手工登陆查看报错信息
**解决方案**
```sql
stop slave
reset slave all
change master to
start slave
```
#### 4.3.2 请求Binlog
**binlog没开**：binlog 损坏，不完整
**reset master** : 从库必崩
![72a85e734fab572a40bb3ce86a9818b3.png](evernotecid://D69BA30F-DB9C-4419-8910-244F110AB8D8/appyinxiangcom/17184459/ENResource/p76)
> 主库执行 reset master;

![9e28adcbeb6aa5cfafc1483e3d28a815.png](evernotecid://D69BA30F-DB9C-4419-8910-244F110AB8D8/appyinxiangcom/17184459/ENResource/p77)

> 从库恢复 重新 CHANGE MASTER TO

```sql
stop slave;

reset slave all;

CHANGE MASTER TO
MASTER_HOST='192.168.17.201',
MASTER_USER='repl',
MASTER_PASSWORD='123',
MASTER_PORT=3307,
MASTER_LOG_FILE='mysql-bin.000001',
MASTER_LOG_POS=154,
MASTER_CONNECT_RETRY=10;

start slave;
```

##### 存储binlog到relaylog

#### 4.3.3 SQL线程故障
relay-log损坏
回放relay-log(主库执行得sql语句，在从库中重新执行一遍)

##### SQL线程故障
**故障背景**
用户在从库创建了dbtest库后，又在从库创建了dbtest的库，此时在主库进行数据的写入，从库将不会在就行复制；原因从库已经存在了dbtest库；

**解决故障**
一切为主库为准；
```sql
# 删除dbtest

# 重启从库
start slave
```
##### 避免SQL线程故障

###### 从库只读
```sql
mysql> show variables like '%read_only%';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_read_only      | OFF   |
| read_only             | OFF   |             ******
| super_read_only       | OFF   |           ******
| transaction_read_only | OFF   |
| tx_read_only          | OFF   |
+-----------------------+-------+
5 rows in set (0.01 sec)
```
###### 使用读写分离中间件



### 4.4 主从复制延时

#### 4.4.1 主从延时监控及原因
##### 主库
1. blinglog写入不及时，sync_binlog参数相关，当`sync_binlog=1`的时候binglog每次提交事务会立即写入磁盘，当`sync_binlog=0`的时候会根据操作的系统的繁忙程度决定是否写入磁盘
https://www.bilibili.com/video/BV1xJ41197iY?p=616





## 备份恢复

### 备份的介绍

### 备份的工具

### 备份的类型

热备 ：对于业务影响最小 InnoDB

温备 ：长时间锁表备份 MyISAM

冷备 ：业务关闭情况下备份

### mysqlump

#### 连接数据库

+ -u
+ -p
+ -S
+ -h
+ -P

#### 基础备份参数

- -A : 全备

```sql
mysqldump -uroot -p -A >/backup/full.sql
```

- -B ：库备份

```
mysqldump -uroot -p -B dbtest mysql > /backup/dbtest.sql
```

- 表备份

```
mysqldump -uroot -p dbtest dbms_instancegroup_redis > /backup/table.sql
```



#### 特殊备份参数

- -R : 存储过程和函数
-






## MHA高可用











https://www.bilibili.com/video/BV1xJ41197iY?p=516


[toc]
