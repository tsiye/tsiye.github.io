---
layout: post
title: Python中yield的使用 
category: Python
tags: Python 
keywords: Python,yield
---
## 可迭代对象(iterable)
当我们用一个列表生成式来建立一个列表时，就建立了一个可迭代对象。
```python
>>> mylist = [x*x for x in range(3)]
>>> for i in mylist :
...    print(i)
0
1
4
```
所有可以使用`for...in...`语法的叫做一个迭代器:列表，字符串，文件。
**but**，读取到的元素全部都存放在了内存中，如果有大量的数据，将会很不方便。
`for x in range(x)`生成一个list
`for x in xrange(x)`生成一个iterable

## 生成器generator
生成器是可以迭代的，但是并不会存储数据。因此我们只能读取他一次。
```python
>>> mylist = (x*x for x in range(3))
>>> for i in mylist :
...    print(i)
0
1
4
```
将`[]`换成了`()`，但是我们只能使用一次。再次使用的话将得不到任何结果。

## yield关键字
> yield是一个类似return的关键字，只是这个函数和返回的是一个**生成器**
当我们调用返回生成器的函数的时候，函数内部的代码并不会立马执行，这个函数只是返回一个生成器对象。
每一轮迭代函数都会执行，从开始到yield，然后返回yield后的值作为第一次迭代的返回值。**下一次迭代时，将从yield的下一行开始进行**

- 含有yield的函数就会变成一个生成器。生成一个generator看起来像函数调用，但不会执行任何的代码，知道对其调用next()(for循环会自动调用next())

## reference
- [Python yield 使用浅析](https://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/index.html)
- [彻底理解Python中的yield](https://www.jianshu.com/p/d09778f4e055)
- [3. (译)Python关键字yield的解释(stackoverflow)](https://pyzh.readthedocs.io/en/latest/the-python-yield-keyword-explained.html)
