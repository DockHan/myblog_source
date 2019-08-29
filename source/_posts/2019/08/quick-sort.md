---
title: quick_sort
categories:
  - 算法导论
tags:
  - 数据结构
  - 算法
type: home
comments: true
date: 2019-08-29 22:43:49
---
## 快速排序
---
### 求解分析
使用分治策略对排序任务进行划分：
- 假设输入数组的 `A[p, r]` 
- 可以将数组划分为 `A[p, q - 1]` 和 `A[q + 1, r]` 使得前一个数组的内容都小于等于A[q],后一个数组的内容都大于等于A[q]
- 这样问题就被划分为两个子问题，继续对子数组进行递归调用的过程
- **合并**: 由于子数组都是原址排序，所以不需要合并操作，即A[p,r]已经是有序的
- 伪代码如下
```python
  def QUICKSORT(A, p, r):
    if p < r:
      q = PARTITION(A, p, r)
      QUICKSORT(A, p, q - 1)
      QUICKSORT(A, q + 1, r)
```
- 算法的关键是PARTITION过程，它实现了对子数组A[p, r]的划分
```python
  def PARTITION(A, p, r):
    # 选取最有一个作为key
    key = A[r]
    # 
    i = p - 1 # 初始时候i 并不存在，用于下面for循环内部进行比较+1
    for j in range(p, r):
      if A[j] <= key:  # j 指向的value都是大于等于key
        i = i + 1
        A[i], A[j] = A[j], A[i]  # i 指向的value都是小于等于key
    A[i+1], A[r] = A[r], A[i+1]
    return i + 1 
```

### python 代码实现
```python
from typing import List


def partition(sort_list: List[int], start_index: int, end_index: int):
    """
    划分待排序数组为连个子数组
    :param sort_list:
    :param start_index:
    :param end_index:
    :return: 分割数组的小标
    """
    key = sort_list[end_index]
    i = start_index - 1
    for j in range(start_index, end_index):  # 这里需要注意 range 中end_index是开区间取值，即end_index - 1
        if sort_list[j] <= key:
            i = i + 1
            sort_list[i], sort_list[j] = sort_list[j], sort_list[i]
    sort_list[i + 1], sort_list[end_index] = sort_list[end_index], sort_list[i + 1]
    return i + 1


def quick_sort(sort_list: List[int], start_index: int, end_index: int):
    """
    快速排序
    :param sort_list: 待排序的列表
    :param start_index: 列表数组的开始下标
    :param end_index: 列表数组的最后一个小标
    :return:
    """
    if start_index < end_index:
        mid_index = partition(sort_list, start_index, end_index)
        quick_sort(sort_list, start_index, mid_index - 1)
        quick_sort(sort_list, mid_index + 1, end_index)


if __name__ == "__main__":
    a = [2, 8, 7, 1, 3, 5, 6, 4] * 100
    quick_sort(a, 0, len(a) - 1)
    print(a)
```

### 算法分析
#### 最坏的情况
- 当每次划分的结果都是所有的数都小于或者大于 key，即数组被划分为n - 1个元素和0个元素时，算法分析公式如下：
  $$T(n) = T(0) + T(n - 1) + \theta(n)$$
- 直观上看，每一次的计算都被累加起来，所以时间复杂度为$\theta(n^{2})$；在最坏的情况下，如果对一个已经排序好的list进行快速排序和插入排序的对比，时间复杂度为快速排序$\theta(n^{2})$, 插入排序为$\theta(n)$.

#### 最好的划分情况
$$T(n) = 2T(\frac{n}{2}) + \theta(n)$$
时间复杂度为: $\theta(n\lg(n))$


### PARTITION的另外一种常见的划分方式(HOARE-PARITION)
- 伪代码如下
```python
  def HOARE_PARITION(A, p, r):
    """
      p, r 是数组A的下标
    """
    key = A[p]  # 选取第一个元素的值作为key
    i = p + 1
    j = r
    while True:
      while A[i] <= key:
        i = i + 1
      while A[j] >= key:
        j = j - 1
      if j > i:
        A[j], A[i] = A[i], A[j]
      else:
        return j  # return j 多多思考
```

#### 代码示例
```python
def hoare_partition(sort_list: List[int], start_index: int, end_index: int):
    """
    常见的方法，两个指针分别指向数组的第2个和最后一个，进行比较和交换，返回划分的index下标
    :param sort_list:
    :param start_index:
    :param end_index:
    :return:
    """
    key = sort_list[start_index]
    i = start_index + 1
    j = end_index

    while True:
        while sort_list[j] >= key and j > start_index:  # j 不能设置大于等于
            j = j - 1
        while sort_list[i] <= key and i < end_index:  # 其中i 不能设置小于等于，防止列表出现重复子串时候出现错误
            i = i + 1

        if j > i:
            sort_list[i], sort_list[j] = sort_list[j], sort_list[i]
        else:
            sort_list[start_index], sort_list[j] = sort_list[j], sort_list[start_index]
            return j
  def quick_sort(sort_list: List[int], start_index: int, end_index: int):
    """
    快速排序
    :param sort_list: 待排序的列表
    :param start_index: 列表数组的开始下标
    :param end_index: 列表数组的最后一个小标
    :return:
    """
    if start_index < end_index:
        # mid_index = partition(sort_list, start_index, end_index)
        mid_index = hoare_partition(sort_list, start_index, end_index)
        quick_sort(sort_list, start_index, mid_index - 1)
        quick_sort(sort_list, mid_index + 1, end_index)
```
  
