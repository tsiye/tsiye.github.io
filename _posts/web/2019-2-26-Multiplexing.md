---
layout: post
title: 网络&操作系统-IO多路复用 select poll epoll 
category: 理解向
tags: [web,OS]
---
## IO多路复用复习
目前支持I/O多路复用的系统调用有 select，pselect，poll，epoll，I/O多路复用就是通过**一种机制，一个进程可以监视多个描述符**，一旦某个描述符就绪（一般是读就绪或者写就绪），能够通知程序进行相应的读写操作。但**select，pselect，poll，epoll本质上都是同步I/O**，因为他们都需要在读写事件就绪后自己负责进行读写，也就是说这个读写过程是阻塞的，而异步I/O则无需自己负责进行读写，异步I/O的实现会负责把数据从内核拷贝到用户空间。
适用与如下场合

## select，poll，epoll
> epoll跟select都能提供多路I/O复用的解决方案。在现在的Linux内核里有都能够支持，其中`epoll是Linux所特有`，而`select则应该是POSIX所规定`，一般操作系统均有实现。
> **POSIX**:可移植操作系统接口(Portable Operating System Interface),X表示对Unix API的传承。IEEE为要在各种UNIX操作系统上运行软件，而定义API的一系列互相关联的标准的总称，其正式称呼为IEEE Std 1003，而国际标准名称为ISO/IEC 9945。

### select
#### 基本原理
> select 函数监视的文件描述符分3类，分别是writefds、readfds、和exceptfds。调用后select函数会阻塞，直到有描述符就绪（有数据 可读、可写、或者有except），或者超时（timeout指定等待时间，如果立即返回设为null即可），函数返回。当select函数返回后，可以通过遍历fdset，来找到就绪的描述符。**select本质上是通过设置或者检查存放fd标志位的数据结构来进行下一步处理**.

#### 优缺点
1. 优点:良好的跨平台支持
2. 缺点: 单个进程能够监视的文件描述符的数量存在最大限制，在Linux上一般为1024，可以通过修改宏定义甚至重新编译内核的方式提升这一限制，但是这样也会造成效率的降低。

### poll 
#### 基本原理
> poll本质上和select没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态，如果设备就绪则在设备等待队列中加入一项并继续遍历，如果遍历完所有fd后没有发现就绪设备，则挂起当前进程，直到设备就绪或者主动超时，被唤醒后它又要再次遍历fd。这个过程经历了多次无谓的遍历。

#### 优缺点
1. 优点:没有最大连接数限制，因为fd是基于链表存储
2. 缺点:大量的fd的数组被整体复制于用户态和内核地址空间之间，而不管这样的复制是不是有意义

### select 和 poll的缺点
同时连接的大量客户端在一时刻可能只有很少的处于就绪状态，因此随着监视的描述符数量的增长，其效率也会线性下降。

### epoll
相对于select和poll来说，epoll更加灵活，没有描述符限制。epoll使用一个文件描述符管理多个描述符，将用户关系的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的copy只需一次。
> epoll支持水平触发和边缘触发，最大的特点在于边缘触发，它只告诉进程哪些fd刚刚变为就绪态，并且只会通知一次。还有一个特点是，epoll使用“事件”的就绪通知方式，通过epoll_ctl注册fd，一旦该fd就绪，内核就会采用类似callback的回调机制来激活该fd，epoll_wait便可以收到通知。

#### 优点
1. 没有最大连接数的限制
2. 效率提升，不是轮询的方式而是监听回调的方式，不会随着fd数目的增加而效率下降。只有活跃的fd才调用callback()函数
3. 内存拷贝。利用mmap()文件映射内存加速与内核空间的消息传递，减少复制开销。

#### 对fd操作的两种模式
1. LT(level triger):默认模式，当epoll_wait检测到描述符事件发生并将此事件通知应用程序，应用程序可以不立即处理该事件。下次调用epoll_wait时，会再次响应应用程序并通知此事件。
同时支持block和no-block socket
2. ET(edge triger):当epoll_wait检测到描述符事件发生并将此事件通知应用程序，应用程序必须立即处理该事件。如果不处理，下次调用epoll_wait时，不会再次响应应用程序并通知此事件。
高速工作方式，只支持no-block socket。减少了epoll被重复时间的触发次数，`必须采用非阻塞套接口`，避免由于一个文件句柄的阻塞把多个处理多个文件描述符的任务饿死。

## 区别
1. 支持一个进程所能打开的最大连接数
![]({{ site.baseurl }}/assets/img/select-poll-epoll.png)
2. FD剧增后带来的IO效率问题
![]({{ site.baseurl }}/assets/img/select-poll-epoll2.png)
3. 消息传递方式
![]({{ site.baseurl }}/assets/img/select-poll-epoll3.png)


## 总结
> 1. 表面上看epoll的性能最好，但是在连接数少并且连接都十分活跃的情况下，select和poll的性能可能比epoll好，毕竟epoll的通知机制需要很多函数回调。
2. select低效是因为每次它都需要轮询。但低效也是相对的，视情况而定，也可通过良好的设计改善。



## reference
[聊聊IO多路复用之select、poll、epoll详解](https://www.jianshu.com/p/dfd940e7fca2)
