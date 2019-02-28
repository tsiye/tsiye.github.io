---
layout: post
title: 语言-Python-元类
category: 理解向
tags: [Python, metaclass]
---
## Python的类
在Python中，类不仅仅是一个抽象的概念，**还是一个对象**，当我们使用关键字class，Python解释器就会创建一个对象。
这个对象（类）自身拥有创建对象（类实例）的能力，而这就是为什么它是一个类的原因。但是，它的本质仍然是一个对象，于是乎你可以对它做如下的操作：
1. 你可以将它赋值给一个变量
2. 你可以拷贝它
3. 你可以为它增加属性
4. 你可以将它作为函数参数进行传递

```python
>>> print ObjectCreator     # 你可以打印一个类，因为它其实也是一个对象
<class '__main__.ObjectCreator'>
>>> def echo(o):
…       print o
…
>>> echo(ObjectCreator)                 # 你可以将类做为参数传给函数
<class '__main__.ObjectCreator'>
>>> print hasattr(ObjectCreator, 'new_attribute')
Fasle
>>> ObjectCreator.new_attribute = 'foo' #  你可以为类增加属性
>>> print hasattr(ObjectCreator, 'new_attribute')
True
>>> print ObjectCreator.new_attribute
foo
>>> ObjectCreatorMirror = ObjectCreator # 你可以将类赋值给一个变量
>>> print ObjectCreatorMirror()
<__main__.ObjectCreator object at 0x8997b4c>
```

## 动态地创建类

## 元类
元类就是动态创建类的东西。函数type实际上就是一个元类

## reference
[深刻理解Python中的元类(metaclass](http://blog.jobbole.com/21351/)
