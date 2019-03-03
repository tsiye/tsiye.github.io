---
layout: post
title: 操作系统-Linux下的环境变量
category: 应用向
tags: web
---
CSDN上看到一篇，写的太好了，感觉就是总结，于是转载一下，谢谢原作者。原链接在文末参考处。

# 环境变量

## 系统级
（1）**/etc/environment**: 是系统在登录时读取的第一个文件，用于为所有进程设置环境变量。系统使用此文件时并不是执行此文件中的命令，而是根据KEY=VALUE模式的代码，对KEY赋值以VALUE，因此文件中如果要定义PATH环境变量，只需加入一行形如PATH=$PATH:/xxx/bin的代码即可。 
（2）**/etc/profile**：是系统登录时执行的第二个文件，可以用于设定针对全系统所有用户的环境变量。该文件一般是调用/etc/bash.bashrc文件。 

/etc/bash.bashrc：系统级的bashrc文件，为每一个运行bash shell的用户执行此文件。此文件会在用户每次打开shell时执行一次。

注意：　/etc/environment是设置整个系统的环境，而/etc/profile是设置所有用户的环境，前者与登录用户无关，后者与登录用户有关。 这两个文件修改后一般都要重启系统才能生效。

## 用户级
（1）~/.profile: 是对应当前登录用户的profile文件，用于定制当前用户的个人工作环境。 
每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。这里是推荐放置个人设置的地方 
（2）~/.bashrc: 是对应当前登录用户的bash初始化文件，当用户每次打开shell时，系统都会执行此文件一次。平时设置这个文件就可以了。

> 那么根据以上描述，这几个文件的执行先后顺序应当是： 
/etc/enviroment –>/etc/profile –>~/.profile –>/etc/bash.bashrc–> ~/.bashrc

# 配置环境变量
只在当前终端有效
## 临时使用
```bash
#终端输入:
export PYTHONPATH=/home/yanting/learning/ssd-caffe/python  #PYTHONPATH设置为该路径
#终端查看一个特定环境变量包含的内容，比如PYTHONPATH
echo $PYTHONPATH
```
## 长期使用
打开第一块内容中的任意一个配置文件(推荐使用./bashrc),在末尾添加
```bash
export PYTHONPATH=/home/yanting/learning/caffe/python:$PYTHONPATH  
# path采用:来分隔,冒号左右不需要空格.
# :$PYTHONPATH在后面新添加的path优先搜索，$PYTHONPATH:在前面说明新添加的path后面搜索，不加代表新路径设置为PYTHONPATH路径。
```
执行`source ~/.bashrc`生效

## reference
[linux下的环境变量](https://blog.csdn.net/jiangyanting2011/article/details/78875928)
