
# 周期性任务 

官网地址：[Periodic Tasks](https://docs.celeryproject.org/en/stable/userguide/periodic-tasks.html)


## 时区

周期性的任务我们要根据所在地区的不同设置与之匹配的时区
```
timezone = "Asia/Shanghai"
```
## 创建应用
创建模块化应用，目录结构如下所示：
```python
tree -I "*.pyc|__pycache__"  proj/ -L 2
proj/
├── celery.py
├── __init__.py
└── tasks.py
```
### 配置周期性任务

示例：周期性任务的配置
```python
app.conf.beat_schedule = {
    'add-every-30-seconds': {
        'task': 'tasks.add',
        'schedule': 30.0,
        'args': (16, 16)
    },
}
```




## 启动 Beat 服务
```python
celery -A proj worker -l INFO
```
注意：启动的时候注意路径
> The celery program can be used to start the worker (you need to run the worker in the directory above proj):


## 启动 celery beat 服务

```python
(py368) [root@lipanpan-celery periodic-tasks]# celery -A proj beat
celery beat v5.0.5 (singularity) is starting.
__    -    ... __   -        _
LocalTime -> 2021-05-03 11:18:32
Configuration ->
    . broker -> redis://:**@172.17.150.44:6379/1
    . loader -> celery.loaders.app.AppLoader
    . scheduler -> celery.beat.PersistentScheduler
    . db -> celerybeat-schedule
    . logfile -> [stderr]@%WARNING
    . maxinterval -> 5.00 minutes (300s)

# 此时的路径结构如下
.
├── celerybeat-schedule
└── proj
    ├── celery.py
    ├── __init__.py
    └── tasks.py
```

## Celery调度程序类

Celery默认的调度程序类是 `celery.beat.PersistentScheduler` 







































