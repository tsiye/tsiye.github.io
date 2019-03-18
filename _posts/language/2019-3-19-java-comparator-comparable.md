---
layout: post
title: Java-comparator & comparable
category: 技术向
tags: [Java]
description: Java
---

## comparable
> comparable 接口定义了 compareTo 方法，用于比较对象。java.lang。
假设要设计求**两个相同类型的对象中较大者**的通用方法。这里的对象可以是两个学生、两个日期、两个圆、两个矩形或者两个正方形。为了实现这个方法，两个对象必须是可比较的。因此！两个对象都该有的共同方法好就是**comparable**。

### 定义
Java提供了Comparable接口。定义如下：
```java
package java.lang;

public interface Comparable<E> {
	public int compareTo(E o);
}
```

### 使用方法
compareTo方法判断这个对象相对于给定对象o的顺序，并且**当这个对象小于、等于或大于给定对象o时，分别返回负整数、0或正整数。**

### 使用范围
Java库中许多类实现了Comparable接口以定义对象的自然顺序。
> Byte, Short, Integer, Long, Float, Double, Character, BigInteger, BigDecimal, Calendar, String, Date.

可以具体看一下Integer中的定义：
```java
public class Integer extends Number implements Comparable<Integer> {
	@override
	public int compareTo(Integer o) {
	}
}
```

### 具体例子
![comparable.png](https://i.loli.net/2019/03/18/5c8f6a69d84bf.png)

### comparable 和 sort
Java API中的java.util.Arrays.sort(Object[])方法就是可以使用compareTo方法对数组中的对象进行比较和排序。
> 那么如果没有实现接口Comparable想用sort肿么办？？？
答曰：可以定义一个新类继承(扩展)自原类并继承Comparable接口

### 小贴士
尽量使compareTo方法与Object类中的equals方法结果一致，虽然现在对object类是否应该compareTo方法尚有争论。

## Comparator接口
> 对于没有实现Comparable的类的对象，我们可以定义一个比较器comparator，java.util.Comparator<T>,来比较**不同类的元素**。

### 定义
```java
public int compare(T element1, T element2)
如果element1 < element2,返回一个负值；element1 == element2，返回 0； element1 > element2, 返回一个正值。
```
我们可以通过重写compare方法来实现比较。

### 具体例子
定义一个比较器类。
![comparator-1.png](https://i.loli.net/2019/03/18/5c8f71bc5be90.png)
GeometricObjectComparator被创建并传递给max方法，程序第17行max方法使用了比较器。
![comparator-2.png](https://i.loli.net/2019/03/18/5c8f71bc67c6d.png)

## Comparable & Comparator
1. 狂妄地一言以蔽之: comparable是类中不同对象的比较，comparator是类间比较。打个比方: comparable 用来比较矩形1和矩形2的面积，comparator用来比较矩形1和圆1的面积。
2. 如果比较的方法只要用在一个类中，用该类实现Comparable接口就可以。
如果比较的方法在很多类中需要用到，就自己写个类实现Comparator接口，这样当要比较的时候把实现了Comparator接口的类传过去就可以，省得重复造轮子。这也是为什么Comparator会在java.util包下的原因。
使用Comparator的优点是：1.与实体类分离 2.方便应对多变的排序规则

## reference
1. [Java中Comparable与Comparator的区别](https://www.jianshu.com/p/fa1a1089d44d)



