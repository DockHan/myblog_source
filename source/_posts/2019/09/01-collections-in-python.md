---
title: 01 collections in python
categories:
  - python
tags:
  - python
  - collections
type: home
comments: true
date: 2019-09-06 22:23:47
---
## collection build-in lib in python
---
python官方对collections的定义：
**collections** -- High-performance container datatypes
collections模块在内置str,list,dict,set,tuple等的基础上提供了额外的数据类型：
- namedtuple(): 生成可以使用名字来访问元素内容的tuple子类
- deque: 双端队列，可以快速的从一侧追加和退出对象
- Counter: 计数器，主要用于计数
- OrderedDict: 有序字典
- defaultdict: 带有默认值的字典

#### namedtuple()
主要用来产生可以使用名称来访问元素的数据对象，通常用来增强代码的可读性， 在访问一些tuple类型的数据时尤其好用
```python
from collections import namedtuple

websites = [
  ("baidu", "http://www.baidu.com", "Liyanhong"),
  ("163", "http://www.163.com", "Dinglei"),
  ("meituan", "http://www.meituan.com", "Wangxing"),
]

websites_named = namedtuple("Website",["name", "url", "founder"])
for site in websites:
  website = websites_named(site)
  print(website)
```

#### deque 双端队列
deque其实是 double-ended queue 的缩写，翻译过来就是双端队列，它最大的好处就是实现了从队列 头部快速增加和取出对象: .popleft(), .appendleft();
你可能会说，原生的list也可以从头部添加和取出对象啊？就像这样：
l.insert(0, v)
l.pop(0)
但是值得注意的是，list对象的这两种用法的时间复杂度是 O(n) ，也就是说随着元素数量的增加耗时呈 线性上升。而使用deque对象则是 O(1) 的复杂度，所以当你的代码有这样的需求的时候， 一定要记得使用deque;
作为一个双端队列，deque还提供了一些其他的好用方法，比如 rotate 等
```python
"""
这个是一个有趣的例子，主要使用了deque的rotate方法来实现了一个无限循环
的加载动画
"""
from collections import deque

fancy_loading = deque('>--------------------')

while True:
    print '\r%s' % ''.join(fancy_loading),
    fancy_loading.rotate(1)
    sys.stdout.flush()
    time.sleep(0.08)
```

#### Counter
Counter
计数器是一个非常常用的功能需求，collections也贴心的为你提供了这个功能。

举个栗子
```python
# -*- coding: utf-8 -*-
"""
下面这个例子就是使用Counter模块统计一段句子里面所有字符出现次数
"""
from collections import Counter

s = '''A Counter is a dict subclass for counting hashable objects. It is an unordered collection where elements are stored as dictionary keys and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or negative counts. The Counter class is similar to bags or multisets in other languages.'''.lower()

c = Counter(s)
# 获取出现频率最高的5个字符
print c.most_common(5)

# Result:
[(' ', 54), ('e', 32), ('s', 25), ('a', 24), ('t', 24)]
```
#### OrderedDict
在Python中，dict这个数据结构由于hash的特性，是无序的，这在有的时候会给我们带来一些麻烦， 幸运的是，collections模块为我们提供了OrderedDict，当你要获得一个有序的字典对象时，用它就对了。

举个栗子
```python
# -*- coding: utf-8 -*-
from collections import OrderedDict

items = (
    ('A', 1),
    ('B', 2),
    ('C', 3)
)

regular_dict = dict(items)
ordered_dict = OrderedDict(items)

print 'Regular Dict:'
for k, v in regular_dict.items():
    print k, v

print 'Ordered Dict:'
for k, v in ordered_dict.items():
    print k, v


# Result:
Regular Dict:
A 1
C 3
B 2
Ordered Dict:
A 1
B 2
C 3
```

#### defaultdict
我们都知道，在使用Python原生的数据结构dict的时候，如果用 d[key] 这样的方式访问， 当指定的key不存在时，是会抛出KeyError异常的。

但是，如果使用defaultdict，只要你传入一个默认的工厂方法，那么请求一个不存在的key时， 便会调用这个工厂方法使用其结果来作为这个key的默认值。

```python
# -*- coding: utf-8 -*-
from collections import defaultdict

members = [
    # Age, name
    ['male', 'John'],
    ['male', 'Jack'],
    ['female', 'Lily'],
    ['male', 'Pony'],
    ['female', 'Lucy'],
]

result = defaultdict(list)
for sex, name in members:
    result[sex].append(name)

print result

# Result:
defaultdict(<type 'list'>, {'male': ['John', 'Jack', 'Pony'], 'female': ['Lily', 'Lucy']})
```