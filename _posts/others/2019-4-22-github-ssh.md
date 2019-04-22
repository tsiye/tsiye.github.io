---
layout: post
title: Git-关于github由https转为ssh
category: 应用向
tags: git
keywords: git
---

进入仓库查看git
```
➜  12306 git:(master) git remote -v         
origin	https://github.com/tsiye/12306.git (fetch)
origin	https://github.com/tsiye/12306.git (push)

```
如果当前是https的，那么可以通过如下命令修改为ssh： 
```
git remote set-url origin git@github.com:account/project.git
```
