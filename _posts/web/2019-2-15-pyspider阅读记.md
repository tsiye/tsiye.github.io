---
layout: post
title: 源码阅读-pyspider源码阅读1 run.py
category: 理解向
tags: spider
description: 阅读pyspider
---
# 架构
> pyspider主要分为Scheduler, Fetcher, Processor, 爬取过程中收到Monitor的监视，抓取的结果被Result Worker处理。
Scheduler负责任务调度，Fetcher负责抓取网页内容，Processor负责解析网页内容，核心部件是拉取网页的fetcher和处理网页的processor。数据持久层问database和result，webui是交互层。

# 库以及组件的理解
1. six模块：专用于兼容Python2 和Python3兼容性的库
2. phantomjs是一个著名的headless browser，提供了大量的API，用于网站的程序化控制。这里用来在fetcher部分解析Javascript网页
3. click: python的一个快速创建命令行的第三方模块，其对于Argparse就好比requests相对于urllib，@click.command()装饰一个函数，使之成为命令行的节后，@click.option()添加命令行选项。[欢迎查阅 Click 中文文档](https://click-docs-zh-cn.readthedocs.io/zh/latest/)
4. xmlrpc:使用http协议作为传输协议的rpc机制。
5. logging模块: 一个日志模块

# 语法的理解
1. python中`*args`相当于是一个tuple，`**kargs`相当于是一个dictionary, ctx貌似是传递上下文内容的一个tuple
2. [import,reload,__import__区别](https://blog.csdn.net/five3/article/details/7762870)
	- import导入一个python模块，将对该模块的内存地址给引用到本地变量环境
	- reload是重新加载模块，在其之前必须import过(不支持from...import的格式)
	```
	import sys 
	reload(sys)
	sys.setdefaultencoding('utf-8')
	```
	这里之所以要重新调用因为在import之后被删除了
	- \_\_import\_\_
	与import作用一样，是import的底层，只接受字符串作为参数`import sys == sys = __import__('sys')`

## tornado_fetcher
http请求交由tornado框架中的httpclient完成，这个client是异步的。其他类似的有request、gevent还有Python内置的urllib等。

# run.py
先从主项目的入口点开始分析。

## cli()
函数cli()做了一些准备工作
其创建了一个字典，放入了连接数据库`taskdb`，`projectdb`，`resultdb`的环境变量，message queue的环境信息，将'newtask_queue', 'status_queue', 'scheduler2fetcher', 'fetcher2processor', 'processor2result'放在其中，还创建好了phantomjs的信息，存储在ctx.obj中
```python
ctx.obj = utils.ObjectDict(ctx.obj or {})
ctx.obj['instances'] = []
ctx.obj.update(kwargs)
```
在后面的函数中
```python
g = ctx.obj
g.instances.append(fetcher)
```
基本上就做好了一系列的配置

最后调用all函数
```python
if ctx.invoked_subcommand is None and not ctx.obj.get('testing_mode'):
        ctx.invoke(all)#调用all()函数
    return ctx
```

## scheduler()
1. 一开始先`load_cls`,看到定义
```python
def load_cls(ctx, param, value):
	if isinstance(value, six.string_types):#看value是否为string类型
		return utils.load_object(value)
    return value
```
```python
#utils.py
def load_object(name):
	"""Load object from module"""
	if "." not in name:
        raise Exception('load object need module.object')
    module_name, object_name = name.rsplit('.', 1)#从字符串后面开始分隔，1次
    if six.PY2:
        module = __import__(module_name, globals(), locals(), [utf8(object_name)], -1)
    else:
        module = __import__(module_name, globals(), locals(), [object_name])#__import__用于动态加载模块
    return getattr(module, object_name)
```
传入一个实例`pyspider.webui.app.app`，加载实例app中的属性值

2. 传入cli()中的字典，scheduler = Scheduler(**kwargs)
```python
kwargs = dict(taskdb=g.taskdb, projectdb=g.projectdb, resultdb=g.resultdb,
                  newtask_queue=g.newtask_queue, status_queue=g.status_queue,
                  out_queue=g.scheduler2fetcher, data_path=g.get('data_path', 'data'))
```

添加基本参数

```python
    scheduler.INQUEUE_LIMIT = inqueue_limit
    scheduler.DELETE_TIME = delete_time
    scheduler.ACTIVE_TASKS = active_tasks
    scheduler.LOOP_LIMIT = loop_limit
    scheduler.FAIL_PAUSE_NUM = fail_pause_num
```

3. 如果有xmlrpc的话开一个线程run起来～，可以看到scheduler()是支持分布式部署的
```python
if xmlrpc:
        utils.run_in_thread(scheduler.xmlrpc_run, port=xmlrpc_port, bind=xmlrpc_host)
```

再贴一下utils里面的代码

```python
def run_in_thread(func, *args, **kwargs):
    """Run function in thread, return a Thread object"""
    from threading import Thread
    thread = Thread(target=func, args=args, kwargs=kwargs)
    thread.daemon = True
    thread.start()
    return thread
```

## fetcher()
与`scheduler()`类似，初始化了`fetcher()`,根据需要使用`phantomjs`和`splash`解析动态网页。
可以看到fetcher()是支持分布式部署的。

## processor()
与上面类似，`fetcher()`爬取的数据传入`processor()`,run()

## result worker()
与上面类似

##webui()
webui是一个基于flask写的界面。
首先还是从context中加载各项属性进入`app.config`

## phantomjs()
著名的无头浏览器，一系列配置，之后调用

## all()
开线程或子进程，调度上述所有模块
```python
if run_in == 'subprocess' and os.name != 'nt':
        run_in = utils.run_in_subprocess
    else:
        run_in = utils.run_in_thread

```

```python
#utils.py
def run_in_thread(func, *args, **kwargs):
    """Run function in thread, return a Thread object"""
    from threading import Thread
    thread = Thread(target=func, args=args, kwargs=kwargs)
    thread.daemon = True
    thread.start()
    return thread


def run_in_subprocess(func, *args, **kwargs):
    """Run function in subprocess, return a Process object"""
    from multiprocessing import Process
    thread = Process(target=func, args=args, kwargs=kwargs)
    thread.daemon = True
    thread.start()
    return thread
```
最后用`quit()`调用click的方法关掉所有线程

## bench()
在bench模式下，仅仅使用在内存中的数据库，而非存储在硬盘的数据库，可以看到其中并没有连接数据库的命令。

## one()
> One mode not only means all-in-one, it runs every thing in one process over
    tornado.ioloop, for debug purpose



	
