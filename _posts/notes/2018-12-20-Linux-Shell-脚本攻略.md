---
layout: post
title: 《Linux Shell 脚本攻略》
category: 读书笔记
tags: Linux Shell
---
# Chapter 1 小试牛刀
## 来历
一开始Unix只支持一种交互式shell - sh
后来出现了bash
---
## 在终端显示输出
- shell脚本一般以`shebang`起始,内核看到shebang会识别并执行脚本
~~~
#!/bin/bash
~~~
> shebang其实是sharp-bang的缩写，Unix行话中，用sharp或者hash或者mesh来称呼‘#’，bang来称呼“！”

- 脚本的执行方式有两种
	1. 将脚本作为命令行参数
	`bash myScript.sh`
	2. 授予脚本可执行权限，将其变为可执行文件
~~~bash
chmod 755 myScript.sh #a+x
./myScript.sh.
~~~	
	cmd1;cmd2等价于
	cmd1
	cmd2

- chmod
	1. 八进制语法
	文件或目录的权限位是由9个权限位来控制，每三位为一组，它们分别是文件所有者（User），用户组（Group）以及其它用户（Other）的读、写、执行。
	r 4 读
	w 2 写
	x 1 执行
	`chmod 755`则是将**所有者**权限设为**读，写，执行**，而同组的以及**其他用户**只能**读和执行**
	2. 符号模式
	u  user
	g  group 
	o  others
	a  all 
	\+ 增加权限
	\- 减少权限
	`chmod ug + rw`对所有者以及群组的人可读写
	`chmod a-w`对所有人删除写的权限
- echo & printf
双引号会对里面的字符进行解释(特殊字符需要‘\’转义) 单引号不会
默认情况下，echo会在输出文本的尾部加上换行转义符。
~~~
#!/bin/zsh
#printf.sh
printf "%-5s %-10s %-4s\n"  No Name Mark
printf "%-5s %-10s %-4.2f\n"  1 Sarath 80.3456
printf "%-5s %-10s %-4.2f\n"  2 James 90.9989
printf "%-5s %-10s %-4.2f\n"  3 Jeff 77.564
~~~
`%-5s`指明了一个左对齐且宽度为5的字符串替换，‘-’表示左对齐，如果不加则右对齐。
`%.2f`表示保留两位小数
## 使用变量&环境变量
> shell定义了一些变量，用于保存用到的配置信息——环境变量

-  `env`用于查看当前shell定义的全部环境变量
-  如何查看其他进程的环境变量？
	1. 先使用`pgrep process_name`使用pgrep后接函数名获得进程ID(PID)
	2. `cat /proc/$PID/environ`打印报表
	3. 由于2中的命令彼此之间有null字符\0分隔，形式不易读
	`sudo cat /proc/$PID/environ | tr '\0' '\n'`将cat的输出通过管道传给tr，将其中的\0替换成\n
~~~zsh
➜  shell pgrep mongod
1097
➜  shell cat /proc/1097/environ
cat: /proc/1097/environ: Permission denied
➜  shell sudo cat /proc/1097/environ
[sudo] password for yeyuan: 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binPWD=/LANG=zh_CN.UTF-8SHLVL=1LANGUAGE=zh_CN:zh_SYSTEMCTL_SKIP_REDIRECT=true_=/usr/local/mongodb/bin/mongod                                                                                                           
➜  shell sudo cat /proc/1097/environ | tr '\0' '\n'
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/
LANG=zh_CN.UTF-8
SHLVL=1
LANGUAGE=zh_CN:zh
_SYSTEMCTL_SKIP_REDIRECT=true
_=/usr/local/mongodb/bin/mongod
~~~

**ATTENTION**
var=value 是赋值操作符
var = value 是等量关系测试
