---
layout: post
title: 内存数据库和磁盘数据库的对比
category: database
tags: database
---
#### 在阅读pyspider时发现作者binux区分了两者的概念，特地一探究竟。

> a database management system that primarily relies on main memory for computer data storage. It is contrasted with database management systems that employ a disk storage mechanism.

## 主内存数据库(IMDB, in-memory database)
1. IMDB要比基于磁盘存储的数据库要快，因为RAM中的优化算法以及较少的CPU操作。消除了查询数据的查找时间。
2. 响应时间至关重要的应用程序（例如运行电信网络设备和移动广告网络的应用程序）通常使用主存储器数据库。在回复查询时，它会将数据加载到计算机的RAM中。
3. 所有数据都存储在主存储器中，无需执行磁盘I / O来查询或更新数据。
4. 数据是持久性的或易变的，具体取决于内存数据库产品。
5. 专用数据结构和索引结构假设数据始终在主存储器中。
6. 针对专业工作负载进行了优化; 即通信行业特定的HLR / HSS工作负载。
7. 数据库大小受主内存量的限制。

## 磁盘数据库(on-disk database)
1. 存储在磁盘上的所有数据，磁盘I / O需要时将数据移动到主存储器中。
2. 数据始终保留在磁盘上。
3. 传统的数据结构，如B-Trees，旨在有效地在磁盘上存储表和索引。
4. 几乎无限的数据库大小。
5. 支持非常广泛的工作负载，即OLTP，数据仓库，混合工作负载等。
