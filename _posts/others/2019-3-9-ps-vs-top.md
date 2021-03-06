---
layout: post
title: 操作系统-ps & top
category: 应用向
tags: [bash]
---

## top
> top是一个动态显示过程,即可以通过用户按键来不断刷新当前状态.如果在前台执行该命令,它将独占前台,直到用户终止该程序为止.比较准确的说,top命令提供了实时的对系统处理器的状态监视.它将显示系统中CPU最“敏感”的任务列表


1. 第一行，任务队列信息，同 uptime 命令的执行结果
当前时间　总共运行时间　登录用户数　负载情况(load average)*load average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了。*
2. 第二行，Tasks — 任务（进程） 
运行中 休眠(sleep) stoped zombie(僵尸)
3. 第三行，cpu状态信息
	5.9%us — 用户空间占用CPU的百分比。
	3.4% sy — 内核空间占用CPU的百分比。
	0.0% ni — 改变过优先级的进程占用CPU的百分比
	90.4% id — 空闲CPU百分比
	0.0% wa — IO等待占用CPU的百分比
	0.0% hi — 硬中断（Hardware IRQ）占用CPU的百分比
	0.2% si — 软中断（Software Interrupts）占用CPU的百分比
4. 第四行,内存状态
	32949016k total — 物理内存总量（32GB）
	14411180k used — 使用中的内存总量（14GB）
	18537836k free — 空闲内存总量（18GB）
	169884k buffers — 缓存的内存量 （169M）
5. 第五行，swap交换分区信息
	32764556k total — 交换区总量（32GB）
	0k used — 使用的交换区总量（0K）
	32764556k free — 空闲交换区总量（32GB）
	3612636k cached — 缓冲的交换区总量（3.6GB）
6. 第七行以下：各进程（任务）的状态监控，项目列信息说明如下：
	PID — 进程id
	USER — 进程所有者
	PR — 进程优先级
	NI — nice值。负值表示高优先级，正值表示低优先级
	VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
	RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
	SHR — 共享内存大小，单位kb
	S — 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
	%CPU — 上次更新到现在的CPU时间占用百分比
	%MEM — 进程使用的物理内存百分比
	TIME+ — 进程使用的CPU时间总计，单位1/100秒
	COMMAND — 进程名称（命令名/命令行）

## ps vs top
1. 
- ps看到的是命令执行瞬间的进程信息,而top可以持续的监视
- ps只是查看进程,而top还可以监视系统性能,如平均负载,cpu和内存的消耗
另外top还可以操作进程,如改变优先级(命令r)和关闭进程(命令k)
2. 
- ps主要是查看进程的，关注点在于查看需要查看的进程
- top主要看cpu,内存使用情况，及占用资源最多的进程由高到低排序，关注点在于资源占用情况

