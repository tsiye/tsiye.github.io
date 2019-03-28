---
layout: post
title: Java-IO总览
category: 理解向
tags: [Java, IO]
description: Java
---

> 本篇的起因来源于Java课的作业。output我一直用的是PrintWriter，直到这次作业老师让我们用setout()重定向，再System.out输出...有点纳闷。直接去翻 《think in java》 发现事情还是很复杂的。索性就把整个Java的IO都梳理一遍。
![iostream.png](https://i.loli.net/2019/03/27/5c9b7060879d6.png)

## InputStream 和 OutputStream
> 首先Java 1.0的鼻祖是这两个类。所有的输入有关的类都应该从`InputStream`继承，与输出有关的类都从`OutputStream`继承。

1. InputStream的作用是用来表示从不同数据源产生输入的类。包括: **字节数组，String对象，文件，管道，序列，其他数据源(Internet中的socket编程)**。其中，InputStream类型如下。

```java
ByteArrayInputStream 
//功能:允许将缓冲区当做InputStream使用。 
//构造器参数以及如何使用: 作为一种数据源，与FilterInputStream相连以提供接口。
StringBufferInputStream 
//功能:String转换成InputStream
//构造器参数以及如何使用: 字符串，底层实现用StringBuffer(线程安全)。也可与FilterInputStream装饰器相连。
FileInputStream 
//功能:文件中读取信息
//构造器参数以及如何使用: 字符串，表示文件名，文件，或者文件描述符(FileDescriptor)，也可与FilterInputStream装饰器相连。
PipedInputStream 
//功能:产生用于写入相关管道的数据 
//构造器参数以及如何使用: PipedOutputStream，作为多线程中的数据源，与FilterInputStream装饰器相连。
SequenceInputStream 。
//功能:将两个或者多个InputStream转换成单一的
//构造器参数以及如何使用: 两个InputStream对象或者一个容纳InputStream的容器Enumeration，作为一种数据源，与FilterInputStream装饰器相连。
FilterInputStream 
//装饰器类，待会专门讲。
```
2. OutputStream决定了输出将要去往的目标:**字节数组(不是String)，文件，管道**
其类型如下。
```java
ByteArrayOutputStream 
//功能:内存中创建缓冲区
//构造器参数以及如何使用: 缓冲区初始尺寸。用于指定数据的目的地，将其与FilterOutputStream相连
FileOutputStream
//功能:将信息写入文件
//构造器参数以及如何使用: 字符串，表示文件名，文件，或者文件描述符(FileDescriptor)，也可与FilterOutputStream装饰器相连。
PipedOuputStream
//功能:任何写入其中的信息都会自动作为相关PipedOutPutStream输出，实现管道化。
//构造器参数以及如何使用: PipedInputStream。指定数据的目的地，与FilterOutputStream相连。
FilterOutputStream 
//抽象类，装饰器的接口。
```

## FilterOutputStream 和 FilterInputStream
> Java IO库需要多种不同功能的组合，这就是Java IO库中存在Filter类的所在。 抽象类`Filter`是所有装饰器类的基类，装饰器必须具有和它所装饰对象相同的接口。

FilterInputStream和FilterOutputStream是用来提供装饰器接口来控制特定输入流的两个类，分别从IO类的基类InputStream和OutputStream派生而来。
我们来看一下两个装饰器类。

- FilterInputStream
```java
DataInputStream
//功能:与DataOutputStream搭配使用，这样可以按照可移植的方式从流中读取基本数据类型(int, char, long)。
//构造器参数以及如何使用: InputStream。包含用于都区基本类型数据的全部接口。
BufferedInputStream
//功能:使用其可以防止每次读写都得进行实际写操作。
//构造器参数以及如何使用: InputStream，可以指定缓冲区大小。本质上不提供接口。只不过是向进程中添加缓冲区所必需的。
LineNumberInputStream
//功能:跟踪输入流中的行号，可调用getLineNumber()和setLineNumber(int)
//构造器参数以及如何使用: InputStream。仅增加了行号，需要与接口对象搭配使用。
PushBackInputStream
//功能:能弹出一个字节的缓冲区。因此可以将读到的最后一个字符回退。
//构造器参数以及如何使用: InputStream。通常是编译器的扫描器，可能永远不会用到。
```
- FilterOutputStream
```java
DataOutputStream
//功能:与DataIntputStream搭配使用，这样可以按照可移植的方式向流中基本数据类型(int, char, long)。
//构造器参数以及如何使用: OutputStream。包含用于都区基本类型数据的全部接口。
PrintStream
//功能:用于产生格式化输出，其中DataOutputStream处理数据的存储，PrintStream处理显示
//构造器参数以及如何使用: OutputStream。可以用boolean值指示是否在每次换行的时候清空缓冲区。对OutputStream的final封装？？？
BufferedOutputStream
//功能:使用其可以防止每次发送数据都得进行实际写操作。可以调用flush清空缓冲区
//构造器参数以及如何使用: OutputStream，可以指定缓冲区大小。本质上不提供接口。只不过是向进程中添加缓冲区所必需的。
```
PrintStream最初的目的是为了可视化的打印各种数据类型。使DataOutputStream能可移植地重构他们。
有两个重要的方法，`print()`和`println()`，对他们进行了重载。另外PrintStream可能会有些问题，因为他捕捉了所有的IOExceptions，必须使用checkError自行测试错误状态。另外PrintStream为国际化，不能以平台无关的方式进行换行操作。(在PrintWriter中得到了解决)


