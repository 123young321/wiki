# Celery中的工作流

## 签名函数
函数签名（或者类型签名或方法签名）定义了函数或方法的输入与输出
签名可包含：
+ 参数及参数的类型
+ 返回值及其类型
+ 可能抛出或传出的异常
+ 该方法在面向对象程序中可用性方面的信息

#### 获取函数签名及参数
使用标准库的signature方法，获取函数签名对象；通过函数签名的parameters属性，获取函数参数

```python
from inspect import signature


def foo(value):
    return value


# 获取函数签名
foo_sig = signature(foo)
# 通过函数签名的parameters属性，可以获取函数参数
foo_params = foo_sig.parameters
print(foo_params)
print(dir(foo_params))
```
#### 参数的分类
参数的分类共分为5大类
```


```





## 工作流


https://zhuanlan.zhihu.com/p/130934654
