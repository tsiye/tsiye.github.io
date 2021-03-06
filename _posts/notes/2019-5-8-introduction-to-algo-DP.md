---
layout: post
title: 《算法导论》-动态规划问题
category: 读书笔记
tags: [算法, DP]
description: 读书笔记
---

> 动态规划(DP，dynamic programming)与分治问题相似，都是通过组合子问题来求解原问题(programming是指表格)。
- 分治问题通过将问题划分成互不相交的子问题，递归地求解，最后将解组合起来。
- 反之，动态规划应用于子问题重叠问题，不同子问题具有公共的子子问题。这个时候，分治问题会做很多不必要的工作，而动态规划只会对那些子问题求解一次，保存在一个表格中，大幅度的减小时间复杂度。

动态规划问题通常用来求解最优化问题，这类问题有很多可行解，DP一般只能找到一种。

### 四个步骤
1. 刻画一个最优解的结构特征
2. 递归定义一个最优解的值
3. 计算最优解的值，一般采用自底向上的方法。
4. 利用计算出的信息构造一个最优解。
