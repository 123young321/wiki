
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

## 启动任务
```python
celery -A proj worker -l INFO
```
注意：启动的时候注意路径
> The celery program can be used to start the worker (you need to run the worker in the directory above proj):

## 配置定时任务































