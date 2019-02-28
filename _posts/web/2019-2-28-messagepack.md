---
layout: post
title: 网络-Python-messagePack
category: 应用向
tags: web
---

## MessagePack
> MessagePack是一种计算机数据交换格式。它是表示数组和关联数组等简单数据结构的二进制形式。

> 官方网站:
It’s like JSON.
but fast and small.

## 与其他格式的比较
- MessagePack比JSON更紧凑，但对数组和整数大小施加了限制。另一方面，它允许二进制数据和非UTF-8编码的字符串。在JSON中，映射键必须是字符串，但在MessagePack中没有这样的限制，任何类型都可以是映射键，包括地图和数组等类型，以及YAML等数字。
简单来讲，它的数据格式与json类似，但是在存储时对数字、多字节字符、数组等都做了很多优化，减少了无用的字符，二进制格式，也保证不用字符化带来额外的存储空间的增加。
![]({{ site.baseurl }}/assets/img/msgpack.png)
- 与BSON相比，MessagePack更节省空间。BSON专为快速内存操作而设计，而MessagePack专为通过线路进行高效传输而设计。

## umsgpack
> 用python编写的便携，轻量级MessagePack序列化器和解串器，兼容Python 2，Python 3，CPython，PyPy
```python
import umsgpack
>>> umsgpack.packb([1, True, False, 0xffffffff, {u"foo": b"\x80\x01\x02", \
...                 u"bar": [1,2,3, {u"a": [1,2,3,{}]}]}, -1, 2.12345])
b'\x97\x01\xc3\xc2\xce\xff\xff\xff\xff\x82\xa3foo\xc4\x03\x80\x01\
\x02\xa3bar\x94\x01\x02\x03\x81\xa1a\x94\x01\x02\x03\x80\xff\xcb\
@\x00\xfc\xd3Z\x85\x87\x94'
>>> umsgpack.unpackb(_)
[1, True, False, 4294967295, {u'foo': b'\x80\x01\x02', \
 u'bar': [1, 2, 3, {u'a': [1, 2, 3, {}]}]}, -1, 2.12345]
```

