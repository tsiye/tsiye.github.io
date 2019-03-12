---
layout: post
title: 《Java语言程序设计》关于一些工具数据类型的用法总结
category: 读书笔记
tags: [leetcode, Java]
description: 读书笔记
---

> 主要对于本书一些基本类型以及常用的数据结构做总结。

## 数组(array)
### 基本使用数组
```java
//使用new elementType[arraySize]创建了一个数组，将其引用赋值给arrayRefVar,因此保存的不是一个数组，而是指向数组的引用
elementType[] arrayRefVar = new elementType[arraySize]; 

//创建加初始化
elementType[] arrayRefVar = {value0, value1, ......, value k};

//获取数组的长度，注意，创建数组后就不能再修改其大小
arrayRefVar.length

//访问数组元素，逐个访问一般套一层循环
arrayRefVar[index]

//对于char[] 类型的数组，可以直接打印
char[] city = {'D','a','l','l','a','s'};
System.out.println(city);
```
### 处理数组
```java
// 随机打乱
for (int i = myList.length - 1; i > 0; i--) {
	int j = (int)(Math.random() * (i + 1));
	double temp = myList[i];
	myList[i] = myList[j];
	myList[j] = temp;
}
```

### foreach循环
```java
for (elementType element: arrayRefVar) {
	//process the element
}
```
### 二分查找法
前提是必须先排好序

## 数学包(Math)
1. 指数函数方法
- `Math.pow(a, b)`求指数运算
- `Math.log(x)`返回x的自然底数
- `Math.log10(x)`返回x以10为底的对数
- `exp(x)`返回e的x次方
- `sqrt(x)`对于x > 0，返回平方根
2. 随机数方法
- `Math.random()`返回一个伪随机浮点数[0,1)
- `Math.random() * (max - min) + min`返回一个在max和min之间的浮点数
- [详情见手册](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random)
3. 三角函数方法
```java
sin(radians) //返回以弧度为单位的角度的三角正弦函数值
cos(radians) //返回以弧度为单位的角度的三角余弦函数值
tan(radians)
```
4. 取整方法
```java
Math.ceil(x) //(向上取整x最接近的整数，作为double类型返回)
Math.floor(x) //(向下取整为x最接近的整数)
Math.rint(x) //(x取整为x最接近的整数)
Math.round(x) //(如果x为float，返回(int)Math.floor(x + 0.5);如果x是双精度数，返回(long)Math.floor(x + 0.5))
```
5. min, max, abs方法
```
Math.min(x) //返回最小值
Math.max(x) //返回最大值
Math.abs(x) //返回绝对值
```

## 字符串
### String
> string对象是不可变的

#### 创建String对象
```java
//使用字符串直接量创建
String message = new String("Welcome to Java"); 
String message = "Welcome to Java"; //Java直接将字符串看作String对象

//字符数组创建
char[] charArray = {'G','o','o','d',' ','D','a','y'};
String message = new String(charArray);
```
#### String的简单方法
```java
length() //返回字符串的长度
charAt(index) //返回字符串s中指定位置的字符,注意字符串下标取值在0 ~ s.length() - 1之间
concat(s1) //将本字符串和一个s1连接，返回一个新的字符串 
toUpperCase() //返回一个新的字符串，所有字母大写 
toLowerCase() //返回一个新的字符串，所有字母小写
trim() //返回一个字符串去掉两边的空白字符,`' '`,`\t`,`\f`,`\r`,`\n`
indexOf(ch) //获取字符串出现的第一个ch的下标。如果没有匹配，返回-1
```
字符串连接的简便方法: `+`  `+=`
#### 读取字符串
```java
Scanner input = new Scanner(System.in);
String s = input.nextLine();
```
#### 比较字符串
```java
equals(s1) //若字符串等于s1，返回true
equalsIgnoreCase(s1) //equals不区分别大小写
compareTo(s1) //字符串大于，等于，小于s1，返回一个大于0，等于0，小于0的整数
compareToIgnoreCase // compareTo不区分大小写
startWith(prefix) //若以特定前缀开始，返回true
endWith(suffix) //若以特定后缀结束，返回true
contains(s1) //若s1是该字符串的子串，返回true
```
**Attention:**
比较字符串不能用 `s1 == s2`，这个只是查看是否指向一个对象，但不会告诉你内容是否相同。
应该用`compareTo()`或者`equals()`

#### 获取子字符串
```java
s.charAt(index) //提取单个字母
s.substring(beginIndex) //返回字符串的子串，从beginIndex到结尾
s.substring(beginIndex, endIndex) //从特定位置beginIndex到下标为endIndex - 1的字符。
```
![substring](https://i.loli.net/2019/03/11/5c85f052b5e72.png)

#### 获取子字符(串)的位置
![indexOf](https://i.loli.net/2019/03/11/5c85f11c239a8.png)
#### String和其他类型的相互转化
- string转数字 `int intValue = Integer.parseInt(intString)`,`double doubleValue = Double.parseDouble(doubleString)`
- 数字转int `String s = number + ""`
- 转字符数组 1`toCharArray()`

#### 字符串的替换与分隔
```java
replace(oldChar, newChar) //将字符串中 所有 匹配的字符串替换成新的字符，返回新的字符串
repalceFirst(oldString, newString) //将字符串中第一个匹配到的子字符串替换成新的字符串，返回
replaceAll(oldString, newString) //将字符串中所有匹配到的子字符串全替换，然后返回。
split(delimiter: String) String[] //返回一个字符串数组，包含被分隔符分隔的子字符串集
String[] tokens = "Java#HTML#Perl".split("#");
for (int i = 0; i < tokens.length; i++) {
	System.out.print(tokens[i] + " ");
}
// 显示 Java HTML Perl
```
**注意**，这里返回的是新的字符串，原始字符串并未改变。
#### 正则
#### 字符串和数组之间的转换
```java
//字符串转化为字符数组
String.toCharArray(); 

//字符数组转化为字符串
String str = new String(new char[]{'J','a','v','a'});
String str = String.valueOf(new char[]{'J','a','v','a'});
```

### StringBuilder & StringBuffer
> 一般来说，只要使用字符串的地方，都可以使用StringBuilder和StringBuffer。
单任务访问使用StringBuilder，并发访问用StringBuffer，StringBuffer是线程安全的。

```java
//初始化
StringBuilder() //创建一个容量为16的空字符串构建器
StringBuilder(capacity:int) //构建一个制定容量的字符串构建器
StringBuilder(s:String) //构建一个指定字符串的字符串构建器 

//修改
append() //追加字符数组，字符串均可
delete(startIndex:Int, endIndex:int) //删除指定位置字符
deleteCharAt(index:int) //删除给定索引位置字符
insert(offset:int, XXX) //向指定位置插入
reverse() 
setCharAt() //将制定索引位置设置为新的字符

//其他
toString() //返回一个字符串
capacity() //返回容量
charAt()
length()
substring(startIndex, endIndex) //从startIndex到endIndex-1
trimToSize() //减少用于字符串构建器的存储大小
```

## ArrayList类
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


