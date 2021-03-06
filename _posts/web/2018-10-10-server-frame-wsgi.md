---
layout: post
title: 网络-服务器，框架，应用，中间块详解
category: 理解向
tags: [web,nginx,wsgi，flask]
---
## 应用程序 中间块 服务器

- 对于传统的**客户端(浏览器)-服务器**架构，处理请求过程：
	1. 客户端向服务器发送HTTP请求
	2. 服务器接受并处理请求
	3. 并给客户端返回响应。
- 新型的web架构
客户端从发送一个 HTTP 请求到 Flask 处理请求，分别经过了 web服务器层，WSGI层，web框架层，这三个层次。不同的层次其作用也不同，下面简要介绍各层的作用。
![]({{ site.baseurl }}/assets/img/wsgi1.png)
	- **web应用程序块**：
		web应用程序：对web服务器接收到的数据进行处理
	 	web框架：方便开发web应用程序(Flask，Django)
	- **web中间件**：最早出现的是CGI(Common Gateway Interface)。后来出现了WSGI，Web Server Gateway Interface，Python定义出来的服务器和应用程序之间的简单接口。Servlet是Java定义出来的简单接口。
	^Flask 依赖的 Werkzeug 就是一个 WSGI 工具包，官方文档的定义是 Werkzeug 是为 Python 设计的 HTTP和 WSGI 实用程序库。我们需要注意的是，Flask 自带的 Werkzeug 是用来开发的，并不能用于生产环境，Flask 是 Web 框架，而 Werkzeug 不是 Web框架，不是 Web 服务器，只是一个 WSGI 工具包，它在 Flask 的作用是作为 Web 框架的底层库，它方便了我们的开发。^
	- **web服务器**：最常见的web服务器有，Nginx、Apache，但是这些web服务器没有内置的WSGI实现，需要通过扩展完成。如 Apache，通过扩展模块 mod_wsgi 来支持WSGI，Nginx可以通过代理的方式，将请求封装好，交给应用服务器，比如 uWSGI。
	需要注意的是：uWSGI也可以作为服务器端使用，uWSGI 是一个全站式的托管服务，它实现了应用服务器（支持多种编程语言）、代理、进程管理器、监视器。
	
- 后端的关系
![]({{ site.baseurl }}/assets/img/wsgi2.png)
1. WSGI将Web服务分成两个部分:服务器和应用程序。
2. WGSI服务器只负责与网络相关的两件事：
接收浏览器的HTTP请求、向浏览器发送HTTP应答；而对HTTP请求的具体处理逻辑，则通过调用WSGI应用程序进行。


## Flask Nginx uWSGI
大块的内容上一块已经讲了，再补充一点细节。
1. WSGI, uWSGI, uwsgi 的区别
**uWSGI包括：1. uwsgi协议 2. web server 内置支持协议模块 3. application 服务器协议支持模块 4.  进程控制程序。uWSGI是由C语言写的，性能较高。**
uwsgi 也是一种协议，uWSGI 实现了 uwsgi、WSGI、http 等协议.
uwsgi 是 uWSGI 使用的一个自有的协议，它用4个字节来定义传输数据类型描述。尽管都是协议，uwsgi 和 WSGI 并没有联系，我们需要区分这两个词。
2. uWSGI 和 Nginx 的关系
Nginx 是高效的 Web 服务器和反向代理服务器，可以用作负载均衡（当有 n 个用户访问服务器时，可以实现分流，分担服务器的压力），与 Apache 相比，Nginx 支持高并发，可以支持百万级的 TCP 连接，十万级别的并发连接，部署简单，内存消耗少，成本低，但 Nginx 的模块没有 Apache 丰富。Nginx 支持 uWSGI 的 uwsgi 协议，因此我们可以将 Nginx 与 uWSGI 结合起来。 
> **uWSGI可以起到web服务器的作用， 那么为什么需要Nginx呢？**
最普遍的说法是 Nginx 对于处理静态文件更有优势，性能更好。其实如果是小网站，没有静态文件需要处理，只用 uWSGI 也是可以的。
但是在有以下情况时最好使用Nginx：
1. 服务器被某个IP攻击时，Nginx只需要将其加入黑名单既可，如果只有uWSGI，就需要修改代码。
2. Nginx的特点是**负载均衡**和**HTTP缓存**。如果不知一台服务器，那么基本Nginx就是必选项。
3. 如果服务器主机上运行了PHP，Python多个语言写的多个应用，需要Nginx进行转发。

## reference
1. [PEP 3333 -- Python Web Server Gateway Interface v1.0.1](https://www.python.org/dev/peps/pep-3333/#middleware-components-that-play-both-sides)
2. [WSGI,web服务器,web框架之间的关系](http://luciferarmy.cn/2017/09/16/WSGI_webserver_webframwork/)
