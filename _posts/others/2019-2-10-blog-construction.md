---
layout: post
title: 本技术博客的搭建
category: 应用向
tags: blog
keywords: blog
---

# 建立技术博客的一些过程
> 技术博客肯定是要越简单越好，再加上自己的时间也有限，就先从最容易搭建的github pages开始，建立博客。

## 建立仓库
先在自己的github pages里创建一个`tsiye.github.io`的repository，里面放上jekyll的模版，因为菜和懒，再加上更应该关注的是知识本身，因此我就借鉴了[Peiwen Lu前辈](https://github.com/P233/3-Jekyll)开发的主题以及[yansu前辈](http://yansu.org/)做的修改。

## 域名获得以及解析
域名购买是在GoDaddy网上，解析则放在了GFW内的腾讯的DNSpod。

## 其他改动
除了内容外，加了自己的disqus，google analytic. 代码高亮使用了rouge，在`_includes`头文件下做了改动，在每篇文章的头部使用syntax:XXX就可以选择想要的高亮主题。这里吐槽一下rouge支持的主题实在是太少了！！

## what remains
有些遗憾的是,由于GFW，disqus功能只有科学上网才能看到，想用自己在东京的服务器做一个反向代理。主要思路是利用那个服务器调用disqus的API，将请求参数发送过来，在本地自己另建一个评论框，实现评论功能。主要是自己前端啥也不会，这个要自己设计一个评论框就很僵硬。等以后再说吧。

## reference
[Jekyll使用Rouge主题](https://my.oschina.net/u/934002/blog/871586)

[用Jekyll搭建的Github Pages个人博客](https://www.jianshu.com/p/88c9e72978b4)

