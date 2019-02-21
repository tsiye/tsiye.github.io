---
layout: post
title: pyspider源码阅读
category: web
tags: spider
description: 阅读pyspider
---
# 架构
> pyspider主要分为Scheduler, Fetcher, Processor, 爬取过程中收到Monitor的监视，抓取的结果被Result Worker处理。
Scheduler负责任务调度，Fetcher负责抓取网页内容，Processor负责解析网页内容，
