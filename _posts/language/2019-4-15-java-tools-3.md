---
layout: post
title: Java-《Java语言程序设计》关于一些工具数据类型的用法总结(二) 线性表list
category: 读书笔记
tags: [Java]
description: 读书笔记
---

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

