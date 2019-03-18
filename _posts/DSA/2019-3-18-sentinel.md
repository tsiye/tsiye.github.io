---
layout: post
title: 算法-哨兵
category: 算法
tags: DSA
---

> Wikipedia:In computer programming, a sentinel value (also referred to as a flag value, trip value, rogue value, signal value, or dummy data) is a special value in the context of an algorithm which uses its presence as a condition of termination, typically in a loop or recursive algorithm.
简单来说，哨兵是在循环或迭代算法中用来标志终止条件的值。**实际上，一切为简化边界条件而引入的附加结点(元素)均可称为哨兵。**

> 哨兵是一个哑对象。一般哨兵不存放任何数据，但其结构体与其他有用的元素一致。
正如其字面意思，哨兵是在边界保卫祖国的军人，所以在编程的世界里，哨兵充当着简化边界条件处理的角色。

1. 像leetcode里面常用的dummy哑节点，就可以很方便的省去头指针，通过对哨兵的引用来代替头指针的引用。

2. 又比如扫雷游戏，每次点击一个格子就要扫描相邻的8个格子。当点击到边界的时候，相邻不足8个格子，这时候就可以在设计游戏的时候为边界外圈加上隐藏的一层格子。这样就可以使所有的操作一致化，不需要对边界的条件作出特殊的处理。

3. 又比如快排、插排中的哑节点，可以去掉swap操作，非常巧妙

## 总结：
1. 哨兵基本不能降低数据结构相关操作的渐近时间界，但其可以降低常数因子。
2. 哨兵的设计可以使得代码更简洁，可以省去一些由于边界环境不同而作出的特殊处理。
3. 在某些情况下哨兵可以使得循环语句更紧凑，降低运行时间里n或者n²项的系数。
4. 慎用：哨兵会占用额外的内存。当使用哨兵的开销较大时，有可能会造成严重的浪费。

## reference
1. [排序中的哨兵](https://blog.csdn.net/wangpeng138375/article/details/23272589)
2. [算法优化|说说哨兵(sentinel value)](https://cloud.tencent.com/developer/article/1081376)



