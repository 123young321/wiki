# 环境搭建

## 开始Celery的第一步

在开始学习或者使用celery的过程中你需要做到以下几点

1. 选择 Broker Celery中支持三种方式分别为
2. 安装 Celery

## 安装 Celery

官网安装提供了几种的安装方式

+ 直接 `pip` 安装
+ 捆绑安装
+ 源码安装

### 创建虚拟环境
```python
python3 -m venv py368
```
### 安装

```python
pip install -U Celery
```

## 选择 Broker

### Redis作为Broker

#### 安装celery[redis]依赖
该位置推荐捆绑安装，如果直接运行 `pip install redis` 则安装的最新版本，可能会有版本兼容问题，所以推荐使用捆绑安装会自动解决版本之间的兼容问题
```
pip install -U "celery[redis]"
```
#### Redis作为Broker的基本配置

Broker基本的格式
```python
redis://:password@hostname:port/db_number
```





## 应用
应用celery分为两种情况，分别为示例使用，模块的应用，一般实例应用表示的是小的案例演示

### 实例应用
实例应用就是初始化Celery获取一个实例化对象，该实例对象会有两个参数分别为 `name` 和 `broker`

```py
# 创建一个文件 touch tasks.py
from celery import Celery
app = Celery('tasks', broker='redis://localhost:6379/0')
@app.task
def add(x, y):
    return x + y
```
### 运行celery worker服务器
```
celery -A tasks worker --loglevel=INFO

celery -A tasks worker --loglevel=INFO
/root/py368/lib64/python3.6/site-packages/celery/platforms.py:797: RuntimeWarning: You're running the worker with superuser privileges: this is
absolutely not recommended!

Please specify a different user using the --uid option.

User information: uid=0 euid=0 gid=0 egid=0

  uid=uid, euid=euid, gid=gid, egid=egid,

 -------------- celery@lipanpan v5.0.5 (singularity)
--- ***** -----
-- ******* ---- Linux-3.10.0-693.2.2.el7.x86_64-x86_64-with-centos-7.9.2009-Core 2021-04-25 22:49:02
- *** --- * ---
- ** ---------- [config]
- ** ---------- .> app:         tasks:0x7fc7f4830898
- ** ---------- .> transport:   redis://localhost:6379/0
- ** ---------- .> results:     disabled://
- *** --- * --- .> concurrency: 1 (prefork)
-- ******* ---- .> task events: OFF (enable -E to monitor tasks in this worker)
--- ***** -----
 -------------- [queues]
                .> celery           exchange=celery(direct) key=celery


[tasks]
  . tasks.add

```

### 模块化的应用













Celery 扮演生产者和消费者的角色,

Celery Beat : 任务调度器. Beat 进程会读取配置文件的内容, 周期性的将配置中到期需要执行的任务发送给任务队列.
Celery Worker : 执行任务的消费者, 通常会在多台服务器运行多个消费者, 提高运行效率.
Broker : 消息代理, 队列本身. 也称为消息中间件. 接受任务生产者发送过来的任务消息, 存进队列再按序分发给任务消费方(通常是消息队列或者数据库),可以是 RabbitMQ 、 Redis, 官方推荐 RabbitMQ.
Producer : 任务生产者. 调用 Celery API , 函数, 而产生任务并交给任务队列处理的都是任务生产者.
Result Backend : 任务处理完成之后保存状态信息和结果, 以供查询, 可以是AMQP, Redis, memcached, MongoDB, SQLAlchemy. Django ORM, Apache Cassandra等.






