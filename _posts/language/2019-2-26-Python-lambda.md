---
layout: post
title: Python lambda函数
category: language
tags: Python
---

> 需要一个函数，又不想定义一个函数，所以用lambda匿名函数

## 传入一个参数
```python
a = lambda x:x*x
print(a(3))
```
输出
9
冒号前面为函数的传入参数，返回冒号后面的结果

## 传入两个参数
```python
a = lambda x,y:x+y
print(a(4,6)) 
```
输出 
10

