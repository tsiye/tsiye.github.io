---
layout: post
title: 源码阅读-pyspider源码阅读2 app.py
category: 理解向
tags: spider
description: 阅读pyspider
---
本次阅读内容为web UI模块下的app.py，这是一个用flask部署的完整后端
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
> Jinja2是为python提供的一个功能齐全的模板引擎。Jinja2提供了对unicode的全支持，以及一个可选集成的沙盒运行环境。它使用BSD协议。设计思想来源于Django引擎，增加了安全性。Flask使用jinja2作为框架的模版框架。

4. `from six.moves import builtins` Python3中命名为`builtins`，Python2中为`__builtin__`
存放了Python的内建函数
使用`dir(builtins)`可查看

5. WebDAV
基于Web的分布式编写和版本控制（WebDAV）是超文本传输协议（HTTP）的扩展，有利于用户间协同编辑和管理存储在万维网服务器文档。
简而言之，就是让客户能用客户端(浏览器)在服务器进行文件交换和共享

## 脉络
`run.py`中调用的是run()函数
可以看到在其中进行了一系列的配置。本地端口，服务器名字，还有自检模式等。

## reference
1. [Middlewares	](http://werkzeug.pocoo.org/docs/0.14/middlewares/)
