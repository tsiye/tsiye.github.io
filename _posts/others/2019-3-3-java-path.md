---
layout: post
title: Java环境配置
category: 应用向
tags: [Java]
---

> 也用了一段时间java了，一直用来刷题，也没记录过安装过程，这次打算命令行使用Java，记录一下涉及到的一些方面。

## JDK & JRE & JVM
![pic1](https://i.loli.net/2019/03/03/5c7b7b3a08bb3.png)
- Java Development Kit，Java开发环境，面向Java开发人员。
JDK用来编译Java的源码(sourcecode，.java文件)，变成字节码(byte code, .class文件)。包含Jre以及编译器，调试器。
- Java Runtime Environment，Java虚拟机，面向程序的使用者。
如果安装了JDK，电脑中就会有两套JRE。JAVA_HOME下的JRE是给jdk里用java编写的工具用的。另一个则是普通java文件使用。JRE中包含了虚拟机(JVM)，库函数以及运行Java程序所必须的文件。
- JVM
Java virtual machine

![[pic2](https://i.loli.net/2019/03/03/5c7b7ba927540.png)

## Java编译过程
Java源文件 xxx.java --->javac.exe编译器--->xxx.class文件 （字节码文件，JVM可以看懂的）--->java.exe解释执行器（需CLASSPATH帮助）
--->把xxx.class文件加载到JVM中去运行--->JVM加载在RAM中运行。  
ps：（因此JVM作用：屏蔽各种OS）--->JAVA的跨平台性得以实现！
先进行源码编译(源码编译器)，在进行字节码的执行(JVM)。
![javadebug.gif](https://i.loli.net/2019/03/03/5c7b7ff3deb50.gif)
![jvmdebug.gif](https://i.loli.net/2019/03/03/5c7b7ff409161.gif)

## 各个环境变量配置
1. JAVA_HOME:Java的安装目录，主要是为了后面表示方便。
2. JRE_HOME:指的是JRE的安装目录，tomcat，eclipse需要的java运行环境就是JRE。jre的目录应该设置为classpath但是现在版本的jdk中的java.exe能自动找到jre的安装位置（因为jdk和jre是同时安装的）所以不设置也可以。
3. CLASSPATH:为javac编译器提供查找.class文件的路径。可以再加个lib，表示想要import的类库。

