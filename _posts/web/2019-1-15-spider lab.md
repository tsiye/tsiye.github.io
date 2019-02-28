---
layout: post
title: 网络-spider lab
category: 项目
tags: spider
---



# 爬虫实战 基础知识积累
## MongoEngine v.s. PyMongo
1. PyMongo 是较为低级 MongoDB 的 Python 驱动器， 封装了 MongoDB API， 并通过 JSON 与 MongoDB 通信， 将 MongoDB 的类型映射成 Python 的基本类型
2. MongoEngine 相当于是在 PyMongo 上又封装了一层， 但是性能相差不大。其实是一个Document-Object Mapper (类似于ORM, Object Relationship Mapper，DOM是针对文档性数据库)。PyMongo是将MongoDB中的数据类型映射成Python的基础数据类型， 而MongoEngine是映射成自定义类。
3. PyMongo 上， 人们可以获得更多的控制权， 但是对于一个大系统来说， MongoEngine更加合适
#### reference
[1] https://api.mongodb.com/python/current/tools.html
[2] https://zh.wikipedia.org/wiki/%E5%AF%B9%E8%B1%A1%E5%85%B3%E7%B3%BB%E6%98%A0%E5%B0%84
[3] https://stackoverflow.com/questions/5712857/pymongo-vs-mongoengine-for-django

## Gevent
### 协程，线程，进程
1. 一个应用程序对应一个进程，一个进程一般有一个主线程以及若干个辅助线程。进程是系统进行资源分配和调度的一个独立单位。由于进程比较重量，占据独立的内存，所以上下文进程间的切换开销（栈、寄存器、虚拟内存、文件句柄等）比较大，但相对比较稳定安全。
线程之间是相互平行的。线程是进程的一个实体,是CPU调度和分派的基本单位,它是比进程更小的能独立运行的基本单位。
在线程中间可以开启协程， 让程序在特定的时间内运行。协程是一种用户态的轻量级线程，协程的调度完全由用户控制。
2. But! 协程避免了无意义的调度，减少切换时浪费的时间，但程序员也因此承担起调度的责任，同时协程也失去了标准线程使用CPU的能力。
### Greelets
在gevent中用到的主要模式是Greenlet, 它是以C扩展模块形式接入Python的轻量级协程。 Greenlet全部运行在主程序操作系统进程的内部，但它们被协作式地调度。
### 函数
gevent.spawn()会创建一个新的 Greenlet 协程对象并运行。
gevent.joinall()会等所有传入的 Greenlet 协程结束后再退出

### 猴子补丁 (monkey patching)
由于协程是非阻塞的，但是Python标准库中的socket是阻塞的，DNS解析无法并发，因此不解决这个问题协程将完全没有意义。
对于这个问题有两种解决方案：
- 使用gevent下的socket模块(from gevent import socket)
- 使用猴子补丁(monkey patching) 
~~~
from gevent import monkey; monkey.patch_socket()
~~~
此后socket标准库中的类和方法都会被替换成非阻塞式的，所有其他的代码都不用修改，这样协程的效率就真正体现出来了。Python中其它标准库也存在阻塞的情况，gevent提供了”monkey.patch_all()”方法将所有标准库都替换。
~~~
from gevent import monkey; monkey.patch_all()
~~~
## BeautifulSoup
### 解析器
可以自行选择对页面所采用的解析器
1. bs4的HTML解析器	BeatifulSoup(mk,'html.parser')	bs4库
2. lxml的HTML解析器	BeautifulSoup(mk,'lxml')	pip install lxml
3. lxml的XML解析器	BeautifulSoup(mk,'xml')	同上
4. html5lib的解析器	BeautifulSoup(mk,'html5lib')	pip install html5lib
### 基本元素
1. Tag 
2. Name	`<tag>.name`
3. Attributes	`<tag>.attrs` 字典形式组织
4. NavigableString	`<tag>.string`
5. Comment	标签内的字符串注释
### 用法
1. soup.prettify()可以打印出HTML的标准格式
2. 若要获取`<a>`的内容	
	soup.a将获取第一个a标签
	soup.a.string将获得标签的标题
	soup.a.attrs将获得标签的属性(是一个字典) a.attrs['href']	可以取出url
