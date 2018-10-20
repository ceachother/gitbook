# 模板

{% embed url="https://blog.csdn.net/han\_\_\_\_shuai/article/details/75252245" %}

九章算法二分查找部分的ladder，前后刷了3,4遍（因为靠前，所以它被刷的次数最多了。）下面也该总结下了。本帖主要包括两方面的内容，一就是二分查找模板，二就是如何结合具体题型来使用二分查找模板。

一.从一个模板谈二分查找

这个模板是上九章算法课程中学到，对于解决问题实战性很强。模板如下：  
例如 lintcode 中的 Classical Binary Search

```python
def findPosition(self, A, target):
    if len(A) == 0:
        return -1
    start = 0
    end = len(A) - 1
    # 留空    
    while start + 1 < end:
        mid = start + (end - start) / 2
        if A[mid] == target:
            return mid
        if A[mid] < target:
            start = mid
        else:
            end = mid
            # 二次判定	
    if A[start] == target:
        return start
    if A[end] == target:
        return end
    return -1
```

与传统的二分查找相比，该方法的最大差别就是不在while中就直接确定出最优解，而是通过 start + 1 &lt; end, 保留两个可能的情况，然后再进行判定。  
这种方式的好处是增加了算法的适用性，减少出错。

二、二分查找相关问题分类

1.找最前，最后位置，或者区间

例如：Last Position of Target, First Position of Target, Search for a Range  
这类问题通常是排序好的数组中存在重复的元素。那么只需对模板进行很小的改动就可以。这里面注意的点，就是求解完mid后，接下来怎么处理。特别是找mid这一刀切到的位置刚好和target相同怎么办。如果是求最前面，则需要让end = target。 因为这样会使下一轮的区间位于相对靠前的位置。同理，对于找靠后的位置，只需要让start = mid 即可。同时，在最后二次判定的时候我们也应该采用相同的道理，如果如果为了找靠前则先判断start，如果找靠后则先判断end。

注意：Closest Number in Sorted Array 这里找最接近位置问题，也可以采用这种方法. 只是在最后增加了判断，start和end谁靠近target就可以了。

2.Rotated Array 相关问题

例如： Find Minimum in Rotated Sorted Array，

```python
def findMin(self, num):
    if len(num) <= 1:
        return num
    start = 0
    end = len(num) - 1
    start_value = num[0]
    end_value = num[end]
    while start + 1 < end:
        mid = start + (end - start) / 2
        if num[mid] > start_value:
            start = mid
        elif num[mid] < end_value:
            end = mid
        result = 0
        if num[start] < num[end]:
            result = num[start]
        else:
            result = num[end]
        result = min(min(result, start_value), end_value)
    return result
```

这题只需要讨论切下去那一刀的位置，相对于旋转点的关系即可。而这个位置可以通过这个旋转后的array 起始点和重点的位置判断。 以为需要注意的情况，是如果序列没有旋转，以及倒序的情况。这两种情况，可在尾部进行判断

Search in Rotated Sorted Array

```python
def search(self, A, target):
    if (len(A) == 0):
        return -1
    start = 0
    end = len(A) - 1
    start_value = A[start]
    end_value = A[end]
    while start + 1 < end:
        mid = start + (end - start) / 2
        if target > start_value:
            if start_value < A[mid] < target:
                start = mid
            else:
                end = mid
        else:
            if target < A[mid] < end_value:
                end = mid
            else:
                start = mid
        if A[start] == target:
            return start
        if A[end] == target:
            return end
    return -1
```

这个问题相对要复杂些，因为不是找最小值，而是找一个具体的目标值，因此要分两大类，共四小类分别讨论。两大类是指target在旋转点左边，和在右边的情况。以及在这两种情况下切分点在target左边和右边的情况。

3.其他技巧

技巧一，利用数据的特殊性，二维一维转换  
例如： Search a 2D Matrix Search a 2D Matrix II

需要利用题目给出数据的排序特征，将矩阵拉长，并借用取余数和整除来确定所对应的二维坐标。代码如下：

技巧二，对中点所对应的值进行抽象和变化

这里中点所对应的值变成了一个复杂的函数，往往需要通过一个helper函数来实现。不过整体框架是一致的。例如：wood cut

```python
def woodCut(self, L, k):
    if len(L) == 0:
        return 0
    max_value = L[0]
    for item in L:
        max_value = max(max_value, item)
        start = 0
        end = max_value
    while start + 1 < end:
        mid = start + (end - start) / 2
        if self.cutable(mid, L, k):
            start = mid
        else:
            end = mid
        if self.cutable(end, L, k):
            return end
    return start


def cutable(self, value, L, k):
    cout = 0
    for item in L:
        cout += item / value
        return cout >= k
```

技巧三，大数据查询的倍增法

该方法是不知道数据长度的情况下，为了快速查找target，先通过倍增的方式来确定查找范围，再进行二分查找，这与STL vector容量的倍增如出一辙。  
对应题目Search in a Big Sorted Array

