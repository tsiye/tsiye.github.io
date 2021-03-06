---
layout: post
title: Java-Serializable
category: 理解向
tags: [Java,Serializable]
description: Java Serializable 接口
---
> java.io.Serializable，表示序列化，是一个空接口，也就是说这个接口没有声明任何的方法，所以实现这个接口的类也就不需要实现任何的方法。

## 序列化和反序列化
> 把原本在内存中的对象状态 变成可存储或传输的过程称之为序列化。序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上。

序列化前的对象和反序列化后得到的对象，内容是一样的(且对象中包含的引用也相同)，但两个对象的地址不同。换句话说，序列化操作可以实现对任何可Serializable对象的”深度复制（deep copy）”。

## 需要序列化的情况
1. 当你想把的内存中的对象状态保存到一个文件中或者数据库中，以便可以在以后重新创建精确的副本；
2. 当你想用套接字在网络上传送对象的时候(从一个应用程序域发送到另一个应用程序域中)；
3. 当你想通过RMI传输对象的时候；


## 序列化ID
序列化 ID在 Eclipse 下提供了两种生成策略，一个设为固定的 1L，另一个是随机生成一个不重复的 long 类型数据（实际上是使用 JDK 工具生成）。一般如果没有特殊需求，用默认的 1L 就可以，这样可以确保反序列化成功。因为不同的序列化id之间不能进行序列化和反序列化。

## transient
整个对象全部序列化了，比如说类里有些数据比较敏感，不希望序列化，一个方法可以用transient来标识，另一个方法我们可以在类里重写