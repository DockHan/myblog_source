---
title: c++ primer 变量和基本类型
categories:
  - c plus plus primer
tags:
  - c plus plus
type: home
comments: true
date: 2019-08-27 10:30:44
---

## 变量和基本数据类型
---
### 内置类型的机器级表示
1. 通常一个内置类型，比如int在内存中(32bit的处理器)，一般占用4个字节的存储空间（也称为Word), So 取值范围是 $1- 2^32 ----- 2^32$，如果一段for循环如下，会进入死循环：
  ~~~
  for(unsigned int i = 0; i < xxx; i++)
  {
    // 当i 超过int的最大范围时， i值%int.MaxValue取模，导致进入死循环
  }
  ~~~
2. 变量的初始化
  复制初始化和直接初始化，如下
  ~~~
  int a = 10;  // 复制初始化
  int b(1024); // 直接初始化
  ~~~

3. 尽量使用const
  const修饰的变量不可被修改，因此必须在定义的时候就进行初始化
  ~~~
  const int PI = 3.1415926; // PI 不可再被程序的其他位置修改其值
  ~~~
4. 引用 reference
  - 就是对象的另外一个名字，在实际应用中主要用作函数的形式参数；
  - 引用是一种复合类型(compound type)，通过在变量名称前添加&符号来定义
      ~~~
      int ival = 1024;
      int &ref_val = ival; // OK , ref_val refers ival
      int &ref_val2;       // error, a reference must be initialized
      int &ref_val3 = 10; // error, initializer must be an object
      ~~~
  - 引用其实就是一个对象的别名，所有作用在该引用上的操作都会对该引用的对象进行影响。
5. **const 引用**
   - 即可以读取，但是不可修改引用对象的内容 
6. typedef 名字
   - 为了隐藏特定类型的实现，强调类型使用的目的
   - 为了简化复杂类型的定义，使其更容易理解
   - 运行一种类型用于多个目的，同事使得每次改类型的目的
7. Enum 枚举
   


