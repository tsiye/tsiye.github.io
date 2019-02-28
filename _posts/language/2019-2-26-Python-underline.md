---
layout: post
title: 语言-Python-下划线汇总
category: 技术向
tags: Python
---

## 单前导下划线 _var
> 下划线前缀的含义是告知其他程序员：以单个下划线开头的变量或方法仅供内部使用。 该约定在PEP 8中有定义。 它通常不由Python解释器强制执行，仅仅作为一种对程序员的提示。

## 单末尾下划线 var_
> 单个末尾下划线（后缀）是一个约定，用来避免与Python关键字产生命名冲突。 PEP 8解释了这个约定。
```python
>>> def make_object(name, class):
SyntaxError: "invalid syntax"

>>> def make_object(name, class_):
...    pass
```

## 双前导下划线 __var
双下划线名称修饰对程序员是完全透明的。 
双下划线前缀会导致Python解释器重写属性名称，以避免子类中的命名冲突。
这也叫做名称修饰（name mangling） - 解释器更改变量的名称，以便在类被扩展的时候不容易产生冲突。
```python
lass ManglingTest:
   def __init__(self):
       self.__mangled = 'hello'

   def get_mangled(self):
       return self.__mangled

>>> ManglingTest().get_mangled()
'hello'
>>> ManglingTest().__mangled
AttributeError: "'ManglingTest' object has no attribute '__mangled'"
```

## 双前导和双末尾下划线 __var__
Python保留了有双前导和双末尾下划线的名称，用于特殊用途。 这样的例子有，__init__对象构造函数，或__call__ --- 它使得一个对象可以被调用。

这些dunder方法通常被称为神奇方法 - 但Python社区中的许多人（包括我自己）都不喜欢这种方法。

最好避免在自己的程序中使用以双下划线（“dunders”）开头和结尾的名称，以避免与将来Python语言的变化产生冲突。

## 单下划线 _
按照习惯，有时候单个独立下划线是用作一个名字，来表示某个变量是临时的或无关紧要的。



##reference
[Python中下划线的5种含义](https://blog.csdn.net/tcx1992/article/details/80105645)
