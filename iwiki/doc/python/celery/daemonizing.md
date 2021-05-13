# 守护进程

## 通用初始化脚本
### Systemd启动方式

#### Systemd 的基本认识

一定要认真读一下推荐的这篇文章 [阮一峰的网络日志-Systemd](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

查看版本，当前系统支持 Systemd 的启动方式
```
systemctl --version
systemd 219
+PAM +AUDIT +SELINUX +IMA -APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ -LZ4 -SECCOMP +BLKID +ELFUTILS +KMOD +IDN
```
#### Usage systemd

##### 启动命令
```
systemctl {start|stop|restart|status} celery.service
```
##### 服务文件 celery.service
配置文件路径：`/etc/systemd/system/celery.service:`

示例配置文件：
```
[Unit]
Description=Celery Service
After=network.target

[Service]
Type=forking
User=root
Group=root

EnvironmentFile=/etc/conf.d/celery
WorkingDirectory=/root/celery-code/next-step/proj

ExecStart=/bin/sh -c '${CELERY_BIN} -A $CELERY_APP multi start $CELERYD_NODES \
    --pidfile=${CELERYD_PID_FILE} --logfile=${CELERYD_LOG_FILE} \
    --loglevel="${CELERYD_LOG_LEVEL}" $CELERYD_OPTS'
ExecStop=/bin/sh -c '${CELERY_BIN} multi stopwait $CELERYD_NODES \
    --pidfile=${CELERYD_PID_FILE} --loglevel="${CELERYD_LOG_LEVEL}"'
ExecReload=/bin/sh -c '${CELERY_BIN} -A $CELERY_APP multi restart $CELERYD_NODES \
    --pidfile=${CELERYD_PID_FILE} --logfile=${CELERYD_LOG_FILE} \
    --loglevel="${CELERYD_LOG_LEVEL}" $CELERYD_OPTS'
Restart=always

[Install]
WantedBy=multi-user.target

```

##### 应用配置文件

配置路径：`/etc/conf.d/celery` 该路径必须和服务文件中的 `EnvironmentFile` 保持一致

```python
# Name of nodes to start
# here we have a single node
CELERYD_NODES="w1"
# or we could have three nodes:
#CELERYD_NODES="w1 w2 w3"

# Absolute or relative path to the 'celery' command:
# CELERY_BIN="/usr/local/bin/celery" # py368 为虚拟环境的路径
CELERY_BIN="/root/py368/bin/celery"

# App instance to use
# comment out this line if you don't use an app
CELERY_APP="proj"
# or fully qualified:
#CELERY_APP="proj.tasks:app"

# How to call manage.py
CELERYD_MULTI="multi"

# Extra command-line arguments to the worker
CELERYD_OPTS="--time-limit=300 --concurrency=8"

# - %n will be replaced with the first part of the nodename.
# - %I will be replaced with the current child process index
#   and is important when using the prefork pool to avoid race conditions.
CELERYD_PID_FILE="/var/run/celery/%n.pid"
CELERYD_LOG_FILE="/var/log/celery/%n%I.log"
CELERYD_LOG_LEVEL="INFO"

# you may wish to add these options for Celery Beat
CELERYBEAT_PID_FILE="/var/run/celery/beat.pid"
CELERYBEAT_LOG_FILE="/var/log/celery/beat.log"

```

###### 服务文件 celerybeat.servece
配置文件位置：`/etc/systemd/system/celerybeat.service`

```
[Unit]
Description=Celery Beat Service
After=network.target

[Service]
Type=simple
User=root
Group=root
EnvironmentFile=/etc/conf.d/celery
WorkingDirectory=/opt/celery
ExecStart=/bin/sh -c '${CELERY_BIN} -A ${CELERY_APP} beat  \
    --pidfile=${CELERYBEAT_PID_FILE} \
    --logfile=${CELERYBEAT_LOG_FILE} --loglevel=${CELERYD_LOG_LEVEL}'
Restart=always

[Install]
WantedBy=multi-user.target

```









https://www.cnblogs.com/zydev/p/11732239.html

https://blog.csdn.net/weixin_43790276/article/details/89076494



https://stackoverflow.com/questions/57266711/daemonization-celery-in-production


http://www.10qianwan.com/articledetail/627516.html

