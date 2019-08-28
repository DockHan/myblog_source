---
title: 测试开发Python面试
categories:
  - Python
tags:
  - 面试
  - Python
type: home
comments: true
date: 2019-08-28 22:38:11
---

## 测试开发面试： Python
---
### 背景
- 最近在进行测试开发的面试过程中，习惯性第一个关于Python基本知识的面试题为Python的一个类中 __new__和__init__的却区别和调用顺序，基本上80%的面试者都没有回答上来，因此实际自己也做一个总结：

### 实践
```python
class NewInit(object):
    def __new__(cls, *args, **kwargs):
        print("__new__ called...")
        return object.__new__(cls)

    def __init__(self):
        print("__init__ called....")


if __name__ == "__main__":
    new = NewInit()

'''
console out print
__new__ called...
__init__ called....
可以看出，在实例化NewInit 之前，解释器会先调用__new__方法，创建一个class的instance
==============================
等同于：
return object.__new__(NewInit, *args, **kwargs) 
调用父类的__new__()方法来制造实例
'''
```

再比如
```python
class NewInitInit(NewInit):
  def __init__(self):
    pass


if __name__ == "__main__":
  newnew = NewInitInit()
'''
这里实例newnew,实际上调用的是父类的__new__方法来制造对象：
return NewInit.__new__(cls, *args, **kwargs)
'''
```

### 使用__new__涉及Python的单例模式
1. 引言
  - 如上所描述，当我们实例化一个对象的时候，虽然我们没有编写__new__方法（大部分情况下），但是解释器是先执行了__new__()来实例化对象，然后交给__init__方法执行，因此我们可以基于这个特点实现单例模式：
2. 实现过程

```python
import threading
class Singleton(object):
  _instance_lock = threading.Lock()

  def __init(self):
    pass
  
  def __new__(cls, *args, **kwargs):
    if not hasattr(Singleton, "_instance"):
      with Singleton._instance_lock:
        if not hasattr(Singleton, "_instance"):
          Singleton._instance = object.__new__(cls)
    return Singleton._instance
  
if __name__ == "__main__":
    o1 = Singleton()
    o2 = Singleton()

    print(o1)
    print(o2)

'''
输出结果：
<__main__.Singleton object at 0x000001AE59737C50>
<__main__.Singleton object at 0x000001AE59737C50>
'''
```

3. 当然Python实现单例模式有很多种方法，以上这种是比较推荐的，也是和Java, C#等语言相似的一种写法
   
### 扩展
1. Python中hasattr(), getattr() setattr()的用法
2. hasattr(object, name)
   - 顾名思义：判断object对象中是否存在name属性，这里需要注意的是对Python而言属性包含变量和方法，有则返回True，没有则返回False；需要注意的是name接收的是string类型，无论变量还是方法，传递进去的都是String类型
    ```python
    class A:
    _instance = 'class_variable'

    def __int__(self, name):
        self._name = name

    def get_name(self):
        return self._name


    if __name__ == "__main__":
        print(True if hasattr(A, "_instance") else False)
        print(True if hasattr(A, "get_name") else False)
        print(True if hasattr(A, "_name") else False)
    '''
    True
    True
    False
    '''
    ```
3. getattr(object, name[,default])
   - 获取object对象的属性的值，如果存在则返回属性值，如果不存在分为两种情况，一种是没有default参数时，会直接报错；给定了default参数，若对象本身没有name属性，则会返回给定的default值；如果给定的属性name是对象的方法，则返回的是函数对象，需要调用函数对象来获得函数的返回值；调用的话就是函数对象后面加括号，如func之于func();另外还需要注意，如果给定的方法func()是实例函数，则不能写getattr(A, 'func')()，因为fun()是实例函数的话，是不能用A类对象来调用的，应该写成getattr(A(), 'func')()；实例函数和类函数的区别可以简单的理解一下，实例函数定义时，直接def func(self):，这样定义的函数只能是将类实例化后，用类的实例化对象来调用；而类函数定义时，需要用@classmethod来装饰，函数默认的参数一般是cls，类函数可以通过类对象来直接调用，而不需要对类进行实例化；
    ```python
    class A:
    _instance = 'class_variable'

    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    @staticmethod
    def class_method():
        return "class method"


    if __name__ == "__main__":
        a = A("ddd")
        print(getattr(A, "_instance", "A_instance"))
        print(getattr(A, "not_has_variable", "not_has_variable"))
        print(getattr(a, "get_name")())
        print(getattr(A, "class_method", "error call class method")())

    ''' 
    console out print:
    class_variable
    not_has_variable
    ddd
    class method
    '''
    ```
4. setattr(object, name, value)
   - 给object对象的name属性赋值value，如果对象原本存在给定的属性name，则setattr会更改属性的值为给定的value；如果对象原本不存在属性name，setattr会在对象中创建属性，并赋值为给定的value；
  ```python
  class A:
    _instance = 'class_variable'

    def __init__(self, name):
        self._name = name

    def get_name(self):
        return self._name

    @staticmethod
    def class_method():
        return "class method"


  if __name__ == "__main__":
      setattr(A, '_instance', [1, 2, 3, 4])
      print(getattr(A, "_instance"))
      setattr(A, 'not_exist_instance', "not_exist_instance")
      print(getattr(A, "not_exist_instance"))
  
  '''
  [1, 2, 3, 4]
  not_exist_instance
  '''
  ```
   



