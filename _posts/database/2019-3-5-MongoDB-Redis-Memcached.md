---
layout: post
title: 数据库-Redis，Memcached,MongoDB比较
category: 技术向
tags: [MongoDB, redis, Memcached, database]
---

今天要讨论的是三者者是not only spl,其中Redis和Mencached是键值存储数据库，mongodb是文档型数据库。

## Memcached和Redis
[也谈谈 Redis 和 Memcached 的区别](http://blog.jobbole.com/101496/)
首先比较一下两个键值存储数据库，这两个相对来说更加适合做缓存。
### Memcached 
从名字就可以看出，memcached是一套**分布式的高速缓存系统**，由LiveJournal的Brad Fitzpatrick开发，但当前被许多网站使用。这是一套开放源代码软件，以BSD license授权发布。
memcached缺乏认证以及安全管制，这代表应该将memcached服务器**放置在防火墙**后。
memcached的API使用三十二比特的循环冗余校验（CRC-32）计算键值后，将数据分散在不同的机器上。当表格满了以后，接下来新增的数据会以LRU机制替换掉。由于memcached通常只是当作缓存系统使用，所以使用memcached的应用程序在写回较慢的系统时（像是后端的数据库）需要额外的代码更新memcached内的数据。
#### 优点
Memcached可以利用**多核优势**，单实例吞吐量极高，可以达到几十万QPS（取决于key、value的字节大小以及服务器硬件性能，日常环境中QPS高峰大约在4-6w左右）。适用于最大程度扛量。
支持直接配置为session handle.
#### 缺点
只支持简单的key/value数据结构，不像Redis可以支持丰富的数据类型。
无法进行持久化，数据不能备份，只能用于缓存使用，且重启后数据全部丢失。
无法进行数据同步，不能将MC中的数据迁移到其他MC实例中。
Memcached内存分配采用Slab Allocation机制管理内存，value大小分布差异较大时会造成内存利用率降低，并引发低利用率时依然出现踢出等问题。需要用户注重value设计。

### Redis
#### 优点
支持多种数据结构，如 string（字符串）、 list(双向链表)、dict(hash表)、set(集合）、zset(排序set)、hyperloglog（基数估算）
支持持久化操作，可以进行aof及rdb数据持久化到磁盘，从而进行数据备份或数据恢复等操作，较好的防止数据丢失的手段。
支持通过Replication进行数据复制，通过master-slave机制，可以实时进行数据的同步复制，支持多级复制和增量复制，master-slave机制是Redis进行HA的重要手段。
单线程请求，所有命令串行执行，并发情况下不需要考虑数据一致性问题。
支持pub/sub消息订阅机制，可以用来进行消息订阅与通知。
支持简单的事务需求，但业界使用场景很少，并不成熟。
#### 缺点
Redis只能使用单线程，性能受限于CPU性能，故单实例CPU最高才可能达到5-6wQPS每秒（取决于数据结构，数据大小以及服务器硬件性能，日常环境中QPS高峰大约在1-2w左右）。
支持简单的事务需求，但业界使用场景很少，并不成熟，既是优点也是缺点。
Redis在string类型上会消耗较多内存，可以使用dict（hash表）压缩存储以降低内存耗用。

## MongoDB
MongoDB 支持丰富的数据表达，索引，最类似关系型数据库，支持的查询语言非常丰富。
### 优点
1. 更高的写负载，MongoDB 拥有更高的插入速度。
2. 处理很大的规模的单表，当数据表太大的时候可以很容易的分割表。
3. 高可用性，设置M-S不仅方便而且很快，MongoDB 还可以快速、安全及自动化的实现节点（数据中心）故障转移。
4. 快速的查询，MongoDB 支持二维空间索引，比如管道，因此可以快速及精确的从指定位置获取数据。MongoDB 在启动后会将数据库中的数据以文件映射的方式加载到内存中。如果内存资源相当丰富的话，这将极大地提高数据库的查询速度。
5. 非结构化数据的爆发增长，增加列在有些情况下可能锁定整个数据库，或者增加负载从而导致性能下降，由于 MongoDB 的弱数据结构模式，添加1个新字段不会对旧表格有任何影响，整个过程会非常快速。
### 缺点
1. 不支持事务。
2. MongoDB 占用空间过大 。
3. MongoDB 没有成熟的维护工具。

## 三者比较
1. 性能
三者的性能都比较高，总的来讲：Memcache 和 Redis 差不多，要高于MongoDB。
2. 便利性
- memcache 数据结构单一。
- redis 丰富一些，数据操作方面，redis 更好一些，较少的网络 IO 次数。
- mongodb 支持丰富的数据表达，索引，最类似关系型数据库，支持的查询语言非常丰富。
3. 存储空间
- redis 在 2.0 版本后增加了自己的VM特性，突破物理内存的限制；可以对key value 设置过期时间（类似 memcache）。
- memcache 可以修改最大可用内存,采用 LRU 算法。`Least Recently Used 近期最少使用算法， 常应用于缓存中的数据淘汰， 其核心思想是“如果数据最近被访问过，那么将来被访问的几率也更高“。`
- mongoDB 适合大数据量的存储，依赖操作系统 VM 做内存管理，**吃内存也比较厉害**，服务不要和别的服务在一起。
4. 可用性
- redis，**依赖客户端来实现分布式读写**；主从复制时，每次从节点重新连接主节点都要依赖整个快照,无增量复制，因性能和效率问题，所以单点问题比较复杂；不支持自动 sharding,需要依赖程序设定一致 hash 机制。一种替代方案是，不用 redis 本身的复制机制，采用自己做主动复制（多份存储），或者改成增量复制的方式（需要自己实现），一致性问题和性能的权衡。
- Memcache 本身没有数据冗余机制，也没必要；对于故障预防，采用依赖成熟的 hash 或者环状的算法，解决单点故障引起的抖动问题。
- mongoDB 支持 master-slave,replicaset（内部采用 paxos 选举算法，自动故障恢复）,auto sharding 机制，对客户端屏蔽了故障转移和切分机制。
5. 可靠性
- redis 支持（快照、AOF）：依赖快照进行持久化，aof 增强了可靠性的同时，对性能有所影响。
- memcache 不支持，通常用在做缓存,提升性能。
- MongoDB 从1.8版本开始采用 binlog 方式支持持久化的可靠性。
6. 一致性
- Memcache 在并发场景下，用 cas 保证一致性。
- redis 事务支持比较弱，只能保证事务中的每个操作连续执行。
- mongoDB 不支持事务。
7. 数据分析
mongoDB 内置了数据分析的功能( mapreduce),其他两者不支持。
8. 应用场景
- redis：数据量较小的更性能操作和运算上。
- memcache：用于在动态系统中减少数据库负载，提升性能;做缓存，提高性能（适合读多写少，对于数据量比较大，可以采用 sharding）。
- MongoDB:主要解决海量数据的访问效率问题。

## reference
1. [简析关系型数据库和非关系型数据库的比较（上）](https://zhuanlan.zhihu.com/p/31140332)
2. [Redis、Memcache和MongoDB的区别](https://www.cnblogs.com/tuyile006/p/6382062.html)
3. [memcache、redis、mongoDB 如何选择？](https://zhuanlan.zhihu.com/p/32940868)
