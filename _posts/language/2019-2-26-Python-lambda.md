---
layout: post
title: 语言-Python-lambda函数
category: 技术向
tags: Python
---

> 函数式编程的基础是lambda表达式。需要一个函数，又不想定义一个函数，所以用lambda匿名函数。

> 官方给出的lambda用途: However, in Python, this is not a serious problem. Unlike lambda forms in other languages, where they add functionality, Python lambdas are only a shorthand notation if you’re too lazy to define a function.  貌似仅仅是用作偷懒

## `lambda <args>: <return-expression>` 
```python
a = lambda x:x*x
print(a(3))
```
输出
9
冒号前面为函数的传入参数，返回冒号后面的结果

## `lambda <args>: <return-expression> if <cond-expression> else <return-expression>`

```python
exp1= lambda x:x+1 if  2==1 else 0
print(exp1(2))
```
输出 
3

## 使用lambda需要注意的细节
```python
fs = [ lambda n: i+n for i in range(5) ]

for k in range(5):
    print "fs[%d]: " %k,fs[k](4)
```
输出
	fs[0]:  8
	fs[1]:  8
    fs[2]:  8
    fs[3]:  8
    fs[4]:  8
没有符合预期，因为我们没有在lambda函数中指定参数i，所以这个时候输入的i为全局变量～

```python
fl = [ (lambda n,i=i: i+n) for i in range(5)]
for k in range(5):
    print "fl[%d]: " %k,fl[k](4)
```
输出
	fl[0]:  4
    fl[1]:  5
    fl[2]:  6
    fl[3]:  7
    fl[4]:  8
