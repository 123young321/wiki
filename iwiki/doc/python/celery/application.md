# Celery 模块化

## 创建模块化应用

项目结构如下所示
```python
tree -L 2 -I "__pycache__"
.
└── proj
    ├── celery.py
    ├── __init__.py
    └── tasks.py

```

项目入口文件 `proj/celery.py`
```python
from celery import Celery

# config 配置文件
# https://docs.celeryproject.org/en/stable/userguide/configuration.html#redis-backend-settings

backend = 'redis://:celery@172.17.150.44:6379/0'
broker = 'redis://:celery@172.17.150.44:6379/1'

app = Celery('proj',
             broker=broker,
             backend=backend,
             include=['proj.tasks'])

# Optional configuration, see the application user guide.
app.conf.update(
    result_expires=3600,
)

if __name__ == '__main__':
    app.start()

```
任务 `proj/tasks.py`

```python
from .celery import app

@app.task
def add(x, y):
    return x + y

@app.task
def mul(x, y):
    return x * y

@app.task
def xsum(numbers):
    return sum(numbers)

```


## 运行任务

运行任务

```python
celery -A proj worker -l INFO
```
## 后台运行

后台启动celery
```python
celery multi start w1 -A proj -l INFO
```
重启Celery
```python
celery  multi restart w1 -A proj -l INFO
```
停止Celery
```python
celery multi stop w1 -A proj -l INFO
```
注意：该命令会使当前正在进行的任务被强制停止，如果想在退出前完成当前正在执行的任务，需要使用 `stopwait`
```python
celery multi stopwait w1 -A proj -l INFO
```
防止Celery被重复启动可以创建 `pid` 和 `log`日志文件

```python
mkdir -p /var/run/celery #
mkdir -p /var/log/celery #
celery multi start w1 -A proj -l INFO --pidfile=/var/run/celery/%n.pid --logfile=/var/log/celery/%n%I.log
```
