---
title: point to offer
categories:
  - 算法导论
tags:
  - 数据结构
  - 算法
  - offer
type: home
comments: true
date: 2019-09-04 23:07:47
---
## 剑指offer题目
---
抽空刷刷题，保持思路敏捷~~~~
1. 使用两个栈实现一个队列，完成队列中pop和push操作；队列中的元素为int类型
   - 解题思路： 有限将元素push到stack1中，第一个元素在栈的最底部，比如push {a, b, c}那么在栈中的顺序是 {c, b, a}，符合栈的特点：先进后出；当需要pop一个元素的时候，a在栈的底部，无法满足队列先进先出的特点，这个时候借助stack2,当stack2为空的时候，将stack1中的元素一次pop，然后一次push到stack2，那么stack2中的顺序为 {a, b, c},刚好满足先进先出的原则，代码如下：
```python
class solution:
  def __init__(self):
    self.stack1 = []
    self.stack2 = []

  def push(self, node):
    self.stack1.append(node)
  
  def pop(self):
    if len(self.stack2) == 0:
      # 只有stack2为空的时候，才将stack1中的value依次pop出来，在push到stack2
      while self.stack1:
        self.stack2.append(self.stack1.pop())  # list.pop() 默认移除列表中最后一个元素，并返回该元素
    return self.stack2.pop()
```

2. 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）,n<=39
   **时间限制1s**，所以不用递归
```python
class Solution:
  def Fibinacci(self, n):
    fibi_list = [0, 1, 1, 2]  # 0: 1, 1 : 1, 2: 1 对应Fib的值
    index = 4
    if n in [0, 1, 2]:
        return 1
    while index <= n:
        fibi_list.append(fibi_list[-1] + fibi_list[-2])
        index = index + 1
    return fibi_list[n]
```

3. 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。
   - 解题思路：本质上还是Fibonacci
     - 当成DP问题来看，最终解是由前面的解累计起来的，如何缩小规模？
     - 首先，1个台阶只有1中可能， 2个台阶由2中可能（一次2个台阶和2次1个台阶）
     - 如果n = 3, 最后跳1步到第三个台阶，则剩余还有2个台阶，可以有2中选择； 最后跳2步到第三个台阶，那么剩余还有1个台阶，只有1中选择
     - **因此，通过分类讨论，问题规模就减少了**
     - 若楼梯台阶 = n; 
       - 最后跳1步到第n个台阶，则剩余n - 2 步没跳，起始跳到n - 2步设它为pre2种
       - 最后跳1步到第n个台阶，则剩余n - 1步没有跳，起始调到n - 1步设它为pre1种
       - 即： dp(i) = dp(i-2) + dp(i-1)
```python
class Solution:
  def jumpFloor(self, number):
      jump = [1, 2]
      for i in range(2, number):
          jump.append(jump[i - 1] + jump[i -2])
          i += 1
      return jump[number - 1]
```
