---
layout: post
title: ubuntu-一些好用的工具
category: 应用向
tags: [ubuntu, linux]
---

## 两个录屏软件
由于Java课程要求...需要给作业录屏...
- kazam 界面极其简单
```shell
sudo apt-get install kazam
``` 
尴尬的是kazam不能给高清屏幕录制全屏，于是选择了SimpleScreenRecord
- SimpleScreenRecord
```shell
添加源：sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
更新源：sudo apt-get update
安装：sudo apt-get install simplescreenrecorder
``` 
## 计算器
> 做结构化学的计算题没带计算器实在是太尬了，手算是不可能的。

- bc
```
$ bc
bc 1.06
Copyright 1991-1994, 1997, 1998, 2000 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'.
scale=4 #限制小数位数
2.53/3.64
.6950
```
也可以在shell中直接输出
```bash
➜  ~ echo "(6+3)*2" |bc 
18
➜  ~ echo "scale=2;15/4" |bc 
3.75
➜  ~ echo "3+4;5*2;5^2;18/4" |bc 
7
10
25
4
```
还有进制转换，字串长度啥的看参考文档吧。
- awk
留着以后用到再说
### reference
1. [linux命令行计算器--bc](https://my.oschina.net/willSoft/blog/39795)

