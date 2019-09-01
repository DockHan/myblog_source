---
title: enumerate in python
categories:
  - python
tags:
  - python
type: home
comments: true
date: 2019-09-01 22:25:16
---
## enumerate用法
- python enumerate()内置函数：
  - 对于一个可迭代的(iterable)/可遍历的对象(list，字符串)等，enumerate将其组成一个索引序列，利用它可以同时拿到index和value
  - 多用于for循环中得到的计数
  - 返回的是一个可enumerate对象
- 语法
```python
enumerate(sequence,[start=0])
# sequence 是一个序列、迭代器和其他支持迭代的对象
# start 下标起始位置
```
---
- 示例
```python
"""
    给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

    你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

    示例:

    给定 nums = [2, 7, 11, 15], target = 9

    因为 nums[0] + nums[1] = 2 + 7 = 9
    所以返回 [0, 1]

    来源：力扣（LeetCode）
    链接：https://leetcode-cn.com/problems/two-sum
    著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
    """
from typing import List
class Solution:
  def two_sum(self, nums: List[int], target: int) -> List[int]:
    value_index_map = {}
    for index, value in enumerate(nums):  # 将数组够着成index, value的形式
      other_num = target - value
      if other_num in value_index_map:
        return [value_index_map[other_num], index]  # 巧妙的实现了： 你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。
      
        value_index_map[value] = index
    return None
```

