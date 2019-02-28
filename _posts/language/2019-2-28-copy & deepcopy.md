---
layout: post
title: 语言-Python-深浅拷贝
category: 理解向
tags: spider
description: 阅读pyspider
---

## 简述
一开始的理解感觉就是：浅拷贝类似C中的指针，仅仅是新建了一个指向该片存储地址的指正。深拷贝的话就是将整片内存全部拷入新的区域。
后来google了一下也确实如此。

> 引用stackoverflow上一位高赞的话: 
Shallow copies duplicate as little as possible. A shallow copy of a collection is a copy of the collection structure, not the elements. With a shallow copy, two collections now share the individual elements.

Deep copies duplicate everything. A deep copy of a collection is two collections with all of the elements in the original collection duplicated.

另一高赞用图说明了问题
- 浅拷贝
![]({{ site.baseurl }}/assets/img/120px-Shallow_copy_done.svg.png)
当我们使用了浅拷贝，仅仅是指针指向发生了变化

- 深拷贝
![]({{ site.baseurl }}/assets/img/120px-Deep_copy_done.svg.png)
深拷贝则是将整片结构连带数据拷入

## 实例
- 一般情况

```python
>>> import copy
>>> origin = [1, 2, [3, 4]]
#origin 里边有三个元素：1， 2，[3, 4]
>>> cop1 = copy.copy(origin)
>>> cop2 = copy.deepcopy(origin)
>>> cop1 == cop2
True
>>> cop1 is cop2
False 
#cop1 和 cop2 看上去相同，但已不再是同一个object
>>> origin[2][0] = "hey!" 
>>> origin
[1, 2, ['hey!', 4]]
>>> cop1
[1, 2, ['hey!', 4]]
>>> cop2
[1, 2, [3, 4]]
#把origin内的子list [3, 4] 改掉了一个元素，观察 cop1 和 cop2
```

- 对于简单的元素，都是一样的
```python
>>> import copy
>>> origin = 1
>>> cop1 = copy.copy(origin) 
#cop1 是 origin 的shallow copy
>>> cop2 = copy.deepcopy(origin) 
#cop2 是 origin 的 deep copy
>>> origin = 2
>>> origin
2
>>> cop1
1
>>> cop2
1
#cop1 和 cop2 都不会随着 origin 改变自己的值
>>> cop1 == cop2
True
>>> cop1 is cop2
True
```

## 小拓展
```python
>>> a = [1, 2, 3]
>>> b = a
>>> a = [4, 5, 6] //赋新的值给 a
>>> a
[4, 5, 6]
>>> b
[1, 2, 3]
# a 的值改变后，b 并没有随着 a 变

>>> a = [1, 2, 3]
>>> b = a
>>> a[0], a[1], a[2] = 4, 5, 6 //改变原来 list 中的元素
>>> a
[4, 5, 6]
>>> b
[4, 5, 6]
# a 的值改变后，b 随着 a 变了
```

在第一段代码中，[4, 5, 6]应该是在空间中新开辟了一片位置，a指向了4所在的位置
在第二段代码中，逐个元素地修改a的值，指针指向没有发生变化

## reference
1. [Python中 copy, deepcopy 的区别及原因](https://iaman.actor/blog/2016/04/17/copy-in-python)
2. [What is the difference between a deep copy and a shallow copy?](https://stackoverflow.com/questions/184710/what-is-the-difference-between-a-deep-copy-and-a-shallow-copy)
