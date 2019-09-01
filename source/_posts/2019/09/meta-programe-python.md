---
title: meta_programe_python
mathjax: true
categories:
  - python
tags:
  - 元编程
  - 装饰器
type: home
comments: true
date: 2019-09-01 20:55:17
---
## Python元编程 & 装饰器
### 装饰器
- 比如，统计一个函数的执行时间
```python
import time
from funtools import wraps
def timestatistic(func):
  """
  Decorator the func's excute times
  """
  @wraps(func)   # @wraps的目的就是不改变被装饰函数的元信息（比如签名和返回值等）
  def wrapper(*args, **kwargv):
    now = time.now()
    result = func(*args, **kwargv)
    print(func.__name__,time.now() - now())
    return result
  return wrapper


@timestatistic
def count_down(n):
  while n > 0:
    n = n - 1

"""
一个装饰器就是一个函数，函数的参数是被装饰的函数，然后返回一个新的函数，上面的例子是对count_down() 函数使用装饰器，等同于如下调用
count_down_new = timestatistic(count_down)
"""
```
- python 内置的一些装饰器比如： @property @staticmethod @classmethod;原理是一样的：
```python
class A:
  @staticmethod
  def method():
    pass

'''
等同于
'''
class A:
  def method():
    pass
  method = staticmethod(method)
```
**其中@wraps装饰器目的是保留被装饰函数的元信息**

- 解除一个装饰器，装饰器通过@wraps修饰后，可以通过调用其魔法函数__wrapped__属性来方法原始方法
```python
@timestatistic
def add(x, y):
  return x + y
origin_add = add._wrapped__
origin_add(3, 5)  # return 8
```

### **带参数的装饰器**
- 比如添加一个可以指定日志级别和日志内容的装饰器
```python
import logging
from functools import wraps
def logged(level, name=None, message=None):
  '''
    add a logged function, level is logging level, name is the logger name, and message is the log  message
  '''
  def decorate(func):
    logname = name if name else func.__module__
    log = logging.getLogger(logname)
    logmsg = message if message else func.__name__
    @wraps(func)
    def wrapper(**args, **kwargv):
      log.log(level, logmsg)
      return func(*args, **kwargv)
    return wrapper
  return decorate


@logged(logging.DEBUG)
def add(x, y):
  return x + y

@logged(logging.CRITICAL, 'example)
def spam():
  print('spam')
```
- 初看起来，这种实现看上去很复杂，但是核心思想很简单。 最外层的函数 logged() 接受参数并将它们***作用在内部的装饰器函数***上面。 内层的函数 decorate() 接受一个函数作为参数，然后在函数上面放置一个包装器。 这里的关键点是包装器是可以使用传递给 logged() 的参数的， 上面调用过程相当于如下代码:
```python
add = logged(logging.DEBUG)(add) = decorate(add) /*中间实例化log处理*/ = wrapper(*args, **kwargv) /*内部进行AOP处理（即log的打印）*/
```

### 可自定义属性的装饰器
- 你想编写一个装饰器，并且允许用户提供参数在运行时控制装饰器行为
- 通过引入一个访问函数，使用nonlocal来修改内部变量
```python
from functools import wraps, partial  # 偏函数partial
import logging
# Utility decorator to attach a function as an attribute of obj
def attach_wrapper(obj, func=None):
    if func is None:
        return partial(attach_wrapper, obj)
    setattr(obj, func.__name__, func)
    return func

def logged(level, name=None, message=None):
    '''
    Add logging to a function. level is the logging
    level, name is the logger name, and message is the
    log message. If name and message aren't specified,
    they default to the function's module and name.
    '''
    def decorate(func):
        logname = name if name else func.__module__
        log = logging.getLogger(logname)
        logmsg = message if message else func.__name__

        @wraps(func)
        def wrapper(*args, **kwargs):
            log.log(level, logmsg)
            return func(*args, **kwargs)

        # Attach setter functions
        @attach_wrapper(wrapper)
        def set_level(newlevel):
            nonlocal level
            level = newlevel

        @attach_wrapper(wrapper)
        def set_message(newmsg):
            nonlocal logmsg
            logmsg = newmsg

        return wrapper

    return decorate

# Example use
@logged(logging.DEBUG)
def add(x, y):
    return x + y

@logged(logging.CRITICAL, 'example')
def spam():
    print('Spam!')

'''
  >>> import logging
  >>> logging.basicConfig(level=logging.DEBUG)
  >>> add(2, 3)
  DEBUG:__main__:add   # 使用默认模块及函数的名称作为msg打印出来
  5
  >>> # Change the log message
  >>> add.set_message('Add called')  # 改变log的message
  >>> add(2, 3)
  DEBUG:__main__:Add called
  5
  >>> # Change the log level
  >>> add.set_level(logging.WARNING)  # 改变log的level
  >>> add(2, 3)
  WARNING:__main__:Add called
  5
  >>>
'''
```


### 扩展
functools.partial是偏函数的意思，目的是将频繁调用函数中的重复内容进行扩展固定，可以查看示例说明： [**偏函数**](http://www.imooc.com/article/255115)

> references: https://python3-cookbook.readthedocs.io/zh_CN/latest/chapters/p09_meta_programming.html


