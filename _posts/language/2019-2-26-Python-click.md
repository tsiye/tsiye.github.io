---
layout: post
title: 语言-Python-click包
category: 技术向
tags: Python
syntax: gruvbox
---
> Click 是Flask团队pallets用 Python 写的一个第三方模块，用于快速创建命令行。

## 坐稳了

## @click.command
在函数前面加上@click.command装饰器就可以把函数变成command对象。
```python
import click

@click.command()
def hello():
    click.echo('Hello World!')#是用echo()为了在python2,3上兼容运行
```
装饰器将函数转化成`command`可以调用的函数
```python
if __name__ == '__main__':
	hello()
```
看起来就想直接调用了一个py文件
	$ python hello.py
	Hello world!

## @click.group
```python
@click.group()
def cli():
    pass

@click.command()
def initdb():
    click.echo('Initialized the database')

@click.command()
def dropdb():
    click.echo('Dropped the database')

cli.add_command(initdb)
cli.add_command(dropdb)
```
`group()`装饰器就像`command()`一样工作，创建一个`group()`对象，可以通过`Group.address——command()`赋予多个可以附加的子命令。
对于简单的脚本，上述示例可以写成下述形式
```python
@click.group()
def cli():
    pass

@cli.command()
def initdb():
    click.echo('Initialized the database')

@cli.command()
def dropdb():
    click.echo('Dropped the database')
```
这里cli()为组命令，下面的initdb()和dropdb()为子命令
通常情况下，如果不调用子命令，就不会调用组命令。
> 如果要执行组命令的话，在cli()前面加上`@click.group(invoke_without_command=True)`就可以不加任何参数运行组命令。

## @click.option & @click.argument
```python
@click.command()
@click.option('--count', default=1, help='number of greetings')
@click.argument('name')
def hello(count, name):
    for x in range(count):
        click.echo('Hello %s!' % name)
```
命令行操作
```
$ python hello.py --help
Usage: hello.py [OPTIONS] NAME

Options:
  --count INTEGER  number of greetings
  --help           Show this message and exit.
```
## 上下文
一个命令如何与嵌套命令对话？**the answer is context**
命令可以通过使用`@pass_context`装饰器来标记自己传递上下文，**上下文作为第一个参数传递**
```python
@click.group()
@click.option('--debug/--no-debug', default=False)
@click.pass_context
def cli(ctx, debug):
    ctx.obj['DEBUG'] = debug

@cli.command()
@click.pass_context
def sync(ctx):
    click.echo('Debug is %s' % (ctx.obj['DEBUG'] and 'on' or 'off'))

if __name__ == '__main__':
    cli(obj={})
```

```
(click) ➜  click python test.py sync  
Debug is off
```
## 怎样调用其他子命令
`ctx.invoke(all)`调用全部`@cli().command()`的命令
`ctx.invoke(XXX)`调用具体子命令 


## reference
[官方文档](https://click-docs-zh-cn.readthedocs.io/zh/latest/commands.html)
