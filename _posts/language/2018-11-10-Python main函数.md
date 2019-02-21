---
layout: post
title: Python main函数
category: language
tags: Python
syntax: gruvbox
---

## 在当前module里的main
1. Python中没有不会像Java一样去找main(),会直接自上而下执行程序

```python
#hello.py
print("first")
 
def sayHello():
    str = "hello"
    print(str);
    print(__name__+'from hello.sayhello()')
 
if __name__ == "__main__":
    print ('This is main of module "hello.py"')
    sayHello()
    print(__name__+'from hello.main')
```

运行结果
> first
This is main of module "hello.py"
__main__from hello.main

可以看见先执行第一行再到`if __name__ == "__main__"`

## if \__name\__ == "\__main\__"
1. \__name\__为内置属性，在当前module中，不管是那个位置\__name\__属性，其值都是\__main\__
2. 当前的文件作为模块被导入其他文件时，\__name\__的值就为当前的py文件名（import hello_world.py, 那么\__name\__就是hello_world）

```python
import hello#上一个例子的hello.py
 
if __name__ == "__main__":
    print ('This is main of module "world.py"')
    hello.sayHello()
    print(__name__)
```

运行结果

> first
This is main of module "world.py"
hello
hellofrom hello.sayhello()
\__main__


-  这样做很好的区分了单个py文件和导入py文件的情况
- 所谓的入口其实也就是个if条件语句，判断成功就执行一些代码，失败就跳过。没有java等其他语言中那样会有特定的内置函数去识别main()方法入口，在main()方法中从上而下执行

[知乎示例](https://www.zhihu.com/question/49136398)

```python
# const.py
PI = 3.14

def main():
print "PI:", PI

main()

# area.py
from const import PI

def calc_round_area(radius):
return PI * (radius ** 2)

def main():
print "round area: ", calc_round_area(2)

if __name__ == "__main__"#如果不加则会同时输出const.py中的main函数
main()	
```
