---
layout: post
title: 网络-服务器限流以及突破服务器限流, 爬虫与反爬
category: 理解向
tags: [web,spider]
---

> 看到头条的一道面试题，故作整理。其实也可以作为爬虫与反爬的答案。

## 服务器限流
如果服务器对访问做限制，大致分为以下几种情况：
1、使用Cookies或者Session来限制；
2、基于注册帐号的限制；
3、验证码限制。
4、在缓存模块记录IP限制；
5、数据库记录IP限制；
6、针对爬虫的话还有利用headers进行限制；

## 关于突破限制无限访问
1. 突破cookies最为简单:不断清除本地的cookies既可，也可以不给HttpWebRequest设置CookiesContainer。
2. 突破注册账号:建立cookie池，随机抽取访问，降低每个账号登录的频率；如果达到上限需要新注册账号，没有验证都比较简单，有验证的话，邮件验证用用个pop3协议或者模拟邮件登陆，得到邮件内容，用激活地址激活。其他的就不细讲了。
3. 突破验证码:常用的就是去除干扰色然后用OCR识别。还有其他滑动验证码，这个可以利用selenium + phantomjs模拟浏览器行为，这基本可以破解绝大多数加密ajax，缺点就是慢。
4. 突破IP限制:使用代理池，或者使用ASDL拨号代理。推荐后者。

##　回到头条的面试题
> 假设现在有一个情景，一些客户端疯狂的访问你的服务器，然后你现在要限制他们的访问，比如说一分钟只准访问100次，怎么实现这个功能，伪代码实现

### 限制对象
如何设置能限制某个IP某一时间段的访问次数是一个让人头疼的问题，特别面对恶意的ddos攻击的时候。其中CC攻击（Challenge Collapsar）是DDOS（分布式拒绝服务）的一种，也是一种常见的网站攻击方法，攻击者通过代理服务器或者肉鸡向向受害主机不停地发大量数据包，造成对方服务器资源耗尽，一直到宕机崩溃。

### 做法
以nginx服务器为例。
要从两个方面入手:
1. `HttpLimitReqModul`用来限制连单位时间内连接数的模块，使用`limit_req_zone`和`limit_req`指令配合使用来达到限制。一旦并发连接超过指定数量，就会返回503错误。
```
http{
    ...

    #定义一个名为allips的limit_req_zone用来存储session，大小是10M内存，
    #以$binary_remote_addr 为key,限制平均每秒的请求为20个，
    #1M能存储16000个状态，rete的值必须为整数，
    #如果限制两秒钟一个请求，可以设置成30r/m

    limit_req_zone $binary_remote_addr zone=allips:10m rate=20r/s;
    ...
    server{
        ...
        location {
            ...

            #限制每ip每秒不超过20个请求，漏桶数burst为5
            #brust的意思就是，如果第1秒、2,3,4秒请求为19个，
            #第5秒的请求为25个是被允许的。
            #但是如果你第1秒就25个请求，第2秒超过20的请求返回503错误。
            #nodelay，如果不设置该选项，严格使用平均速率限制请求数，
            #第1秒25个请求时，5个请求放到第2秒执行，
            #设置nodelay，25个请求将在第1秒执行。

            limit_req zone=allips burst=5 nodelay;
            ...
        }
        ...
    }
    ...
}
```
2. `HttpLimitConnModul`用来限制单个ip的并发连接数，使用`limit_zone`和`limit_conn`指令
http{
    ...

    #定义一个名为one的limit_zone,大小10M内存来存储session，
    #以$binary_remote_addr 为key
    #nginx 1.18以后用limit_conn_zone替换了limit_conn
    #且只能放在http作用域
    limit_conn_zone   one  $binary_remote_addr  10m;  
    ...
    server{
        ...
        location {
            ...
           limit_conn one 20;          #连接数限制

           #带宽限制,对单个连接限数，如果一个ip两个连接，就是500x2k
           limit_rate 500k;            

            ...
        }
        ...
    }
    ...
}


这两个模块的区别前一个是对一段时间内的连接数限制，后者是对同一时刻的连接数限制.

为了保护自己的测试ip以及搜索引擎的spider，可以设置一个白名单。使用map指令映射搜索引擎客户端的ip为空串，如果不是搜索引擎就显示本身真是的ip，这样搜索引擎ip就不能存到limit_req_zone内存session中，所以不会限制搜索引擎的ip访问

## 爬虫与反爬的斗争史
[如何应对网站反爬虫策略？如何高效地爬大量数据? - 申玉宝的回答 - 知乎](https://www.zhihu.com/question/28168585/answer/74840535)
太好笑了，赞一个

## reference
1. [nginx限制某个IP同一时间段的访问次数](https://blog.csdn.net/u010094934/article/details/72697086)
2. [Nginx中如何限制某个IP同一时间段的访问次数和并发数](http://www.52yunwei.net/341.html)
