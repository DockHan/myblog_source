---
title: tree
mathjax: true
categories:
  - 算法导论
tags:
  - 数据结构
  - 算法
type: home
comments: true
date: 2019-08-31 08:09:54
---
## 树
### 基本概念
- 树的高度，也成为了树的深度；比如一个完全二叉树高度为h，能包含的最大元素和最小元素分别是多少？
  ```python
  '''
                |    深度0
            |      |  深度1
          |   |  |   | 深度2
          ... ... ... ... 
  '''
  ```
所以最大元素为$\sum_{r=0}^{h}2^{r}$,最小元素为$\sum_{r=0}^{h}2^{h-1} + 1$
- 二叉树
  - 定义：通过递归的方式来定义二叉树，T是定义在有限节点集上的结构，它或者不包含任何节点，或者包含三个不想交的节点集：一个根节点，一颗称为左子树的二叉树，一个称为右子树的二叉树。
- 满二叉树指的是：所有节点的节点度都为2
- 完全k叉树：所有叶节点的深度相同，且所有内部节点的节点度都为K的k叉树。一个高度为h的完全k叉树的内部节点数目为：
$$1 + k + k^2 + \cdot + k^{h-1} = \sum_{i = 0}^{h -1}K^i = \frac{k^h - 1}{k - 1} $$

### 堆排序
- **堆：是一个数值，可以被看成是一个近似的完全二叉树，树上的每一个节点对应数组中的一个元素。除最底层外，该树是完全满的，而且从左向右填充。**
- 表示堆的数组A包含两个属性：A.length给出数组元素的个数，A.head-size表示有多少个元素存储在该数组中。
```python
'''
                         16
                    /            \  
              14                     10
          /       \                 /   \ 
      8             7              9      3
    /   \          /
  2      4        1
'''
A = [16, 14, 10, 8, 7, 9, 3, 2, 4, 1]   # 对应在数组中的存储结构
```
- 二叉堆可以分为两种形式： 最大堆和最小堆。除了都满足堆的特性外，在最大堆中除了根节点外，所有节点i都要满足：
$$[PARENT(i)] \ge A[i]$$
最小堆则反之, 最小堆通常被用于构造优先队列。
$$[PARENT(i)] \le A[i]$$
- **相关伪代码**
  - MAX_HEAPIFY 用于维护最大堆得重要过程，它输入一个数组A和一个小标，在调用MAX_HEAPIFY的时候，我们假定根节点为LEFT[i]和RIGHT[i]的二叉树都是最大堆，但这是A[i]有可能小于其孩子，就违背了最大堆的特性。 MAX_HEAPIFY通过让A[i]的值在最大堆中逐级下降，从而使得以下标为i的根节点子树重新遵循最大堆的特性。
```python
 def MAX_HEAPIFY(A, i):
  l = LEFT[i]
  r = RIGHT[i]
  if l <= A.head-size and A[l] > A[i]:
    largest = l
  else:
    largest = i
  if r <= A.head-size and A[r] > A[largest]:
    largest = r
  
  if largest != i:
    A[r], A[i] = A[i], A[r]  # swap 
    MAX_HEAPIFY(A, largest)
```
#### **建堆**
- 可以利用自底向上的方法使用MAX_HEAPIFY把一个大小为n = A.length的数组A[1..n]转换为最大堆，伪代码如下：
```python
def BUILD_MAX_HEAD(A):
  A.head-size = A.length
  for i = [A.length/2] downto i:   # [A.length/2]向下取整
    MAX_HEAPIFY(A, i)
```
#### 堆排序算法
- 初始时候，可以先利用BUILD_MAX_HEAD将输入的A[1,...,n]构建成最大堆，其中n = A.length,因为最大的元素总是在A[1]中，因此通过它与A[n]互换，将A[n]从堆中去掉，在重新构建A[1,....,n-1]的最大堆，从而得到一个递增或者递减的排序数组A，代码如下:
```python
def HEAD_SORT(A):
  BUILD_MAX_HEAD(A)
  for i = A.length downto 2:
    swap A[1] with A[i]
    A.head-size = A.head-size -1
    MAX_HEAPIFY(A,1)
```



