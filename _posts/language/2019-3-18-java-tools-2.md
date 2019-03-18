---
layout: post
title: Java-《Java语言程序设计》关于一些工具数据类型的用法总结(二) 合集collection
category: 读书笔记
tags: [leetcode, Java]
description: 读书笔记
---

> Java 合集框架(java.util.collection)支持一下两种类型的容器:
- 一种是为了存储一个元素合集，简称为合集(collection)。
- 另一种是为了存储键值对，简称为映射表(map).

# 合集collecion
- set用于存储一组不重复的元素。
- List用于存储一个有序元素合集。
- Stack用于存储后进先出的方式处理的对象。
- Queue用于存储先进先出方式处理的对象。
- Priority Queue 用于存储按照优先级顺序处理的对象。

![collection.png](https://i.loli.net/2019/03/18/5c8f470fc3790.png)

## 线性表
> List接口继承自collection接口，有两个具体类，ArrayList或者LinkedList来创建线性表。


### ArrayList类
```java
//添加元素
add(E) //加到末尾
add(index:int, E) //加到指定下标处

//清除所有元素
clear() 

//若包含则返回true
contains(o:object)

//获取下标处的元素
get(index:int) 

//返回第一个匹配元素的下标
indexOf(o:object) 

//返回匹配的最后一个元素下标
lastIndexOf(o:object)

isEmpty()

//去除元素,去除则返回true
remove(o:object): boolean
remove(index:int):boolean

//返回列表中的元素个数
size():int

//设定
set(index:int, o:E)
```


