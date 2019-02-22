---
layout: post
title: pyspider源码阅读
category: web
tags: spider
description: 阅读pyspider
---
# 架构
> pyspider主要分为Scheduler, Fetcher, Processor, 爬取过程中收到Monitor的监视，抓取的结果被Result Worker处理。
Scheduler负责任务调度，Fetcher负责抓取网页内容，Processor负责解析网页内容，核心部件是拉取网页的fetcher和处理网页的processor。数据持久层问database和result，webui是交互层。

# 库以及组件的理解
1. six模块：专用于兼容Python2 和Python3兼容性的库
2. phantomjs是一个著名的headless browser，提供了大量的API，用于网站的程序化控制。

## tornado_fetcher
http请求交由tornado框架中的httpclient完成，这个client是异步的。其他类似的有request、gevent还有Python内置的urllib等。
