---
layout: post
title: pyspider源码阅读2 app.py
category: web
tags: spider
description: 阅读pyspider
---
本次阅读内容为web UI模块下的app.py
## 涉及概念整理
1. os.name返回当前操作系统类型，当前只注册了三个值:posix,nt,java。对应linux/windows/java虚拟机
2. @property装饰器主要作用是将一个函数设置成只读对象
```python
@property
def x(self):
    return self._x
```
等价于
```python
def getx(self):
    return self._x
x = property(getx)
```
这上面可以看出，@property修饰的相当于一个getter方法。
下面可以看到这个装饰器还内置了setter方法，birth可以修改，age是只读属性
```python
class Student(object):

    @property
    def birth(self):
        return self._birth

    @birth.setter
    def birth(self, value):
        self._birth = value

    @property
    def age(self):
        return 2014 - self._birth
```
3. jinja
> Jinja2是为python提供的一个功能齐全的模板引擎。Jinja2提供了对unicode的全支持，以及一个可选集成的沙盒运行环境。它使用BSD协议。
