---
layout: post
title: 网络-spider lab2
category: 技术向
tags: spider
---


# spider lab 2
## 问题1
在获取各个子链接时报错：TypeError: _wrap_socket() argument 1 must be _socket.socket, not SSLSocket
经过google以及适当摸索，发现去掉import gevent.monkey无报错。貌似是python 3 中猴子补丁的范围必须是全部。将`gevent.monkey.patch_socket()`改为`gevent.monkey.patch_all()`无报错。
应该是gevent库与requests库出现了冲突

## requests库中 content和text的区别
content返回的是bytes类型的原始数据
text返回的是unicode类型的数据

## 模拟登录
### cookies
- http是无状态的协议，利用存贮在用户本地的cookies，网站可以知道是哪个用户进行访问
- cookies是直接放在http协议的header中进行传输的
- 但是cookies不安全，如果有人截获了请求，很容易就可以分析出用户名密码，为了解决这个隐患，产生了session机制

### session
- 服务器会根据用户名和密码生成一个session ID(SID)，存到服务器的数据库中
用户每次访问时服务器会将对应的session ID发送给用户
浏览器将session ID存到cookies中，作为一个键值项
以后浏览器每次请求都会将含session ID的cookies信息返回给服务器
服务器根据cookies中的session ID去数据库中查询，解析出用户名就知道是哪个用户的请求了
- session 的运行依赖 session ID，而 session ID 是存在 cookie 中的
- session ID是有过期时间的

### session实现
通常session cookie是不能跨窗口使用的，当你新开了一个浏览器窗口进入相同页面时，系统会赋予你一个新的sessionid，这样我们信息共享的目的就达不到了，此时我们可以先把sessionid保存在persistent cookie中，然后在新窗口中读出来，就可以得到上一个窗口SessionID了，这样通过session cookie和persistent cookie的结合我们就实现了跨窗口的session tracking（会话跟踪）。            在一些web开发的书中，往往只是简单的把Session和cookie作为两种并列的http传送信息的方式，session cookies位于服务器端，persistent cookie位于客户端，可是session又是以cookie为基础的，明白的两者之间的联系和区别，我们就不难选择合适的技术来开发web service了。
### 本次模拟登陆的实现过程
- 使用的是Chromium Version 71.0.3578.98 (Official Build) Built on Ubuntu , running on Ubuntu 16.04 (64-bit)
- 首先打开开发者工具，用账号密码进行登录。选上preserve log
![]({{ site.baseurl }}/assets/img/20190211-210427.png)
可以看到截取到两个包
- 第一个包是POST登录表单的，将其中的头部，请求连接以及传输的data复制下来
- 第二个包是GET的新的页面，将其中的头部也复制下来。这里我发现POST文件中的cookies和第二个文件中的cookies不太一样，不知道是如何表示的，只能先依样画葫芦全部复制下来，作为requests的请求参数
- 之后我们就可以看到登陆后才有权限查看的数据

### 实现点击
- 就在欢天喜地地准备爬取数据时，突然发现那些登陆后的才能查看的数据需要点击查看![]({{ site.baseurl }}/assets/img/20190211-211020.png)
- 再翻html树，发现并没有相应的字符......呵呵， 保护的还挺好
![]({{ site.baseurl }}/assets/img/20190211-211147.png)
- 然后发现有一个链接标签下有一个“onclick”属性，试着访问其中的链接，发现可以得到相应的数据，再对页面用BeautifulSoup烧汤。用正则进行操作，截取到相应的数据， 嘿嘿。
