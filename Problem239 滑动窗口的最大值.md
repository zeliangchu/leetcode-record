# Problem239 滑动窗口的最大值

## 题目描述

给定一个整数数组nums,有一个大小为k的滑动窗口从数组的最左端移动到数组的最右端。你只可以看到在滑动窗口内的k个数字，滑动窗口每次只向右移动一位。返回由滑动窗口中的最大值组成的数组。

## 思路1 暴力法

超时

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        #第一种方法 python的切片操作超时
        if k == 1:
            return nums
        res = []
        n = len(nums)
        if k == n:
            return [max(nums)]
        for i in range(n - k + 1):
            res.append(max(nums[i:i+k]))
        return res
```

**复杂度分析**

- 时间复杂度：O(n*k)
- 空间复杂度：O(n-k+1)

## 思路2 使用双端队列，但是双端队列的大小还是窗口大小

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        #第二种方法，使用双端队列，一个队列的长度就是窗口的大小，每次移动窗口的时候队列前端的元素弹出，然后加入一个元素，并维持一个变量来记录队列的最大值。
        from collections import deque
        window = deque()
        if k == 1:
            return nums
        res = []
        n = len(nums)
        if k == n:
            return [max(nums)]
        for i in range(k):
            window.append(nums[i])
        max_value = max(window)
        res.append(max_value)
        for i in range(k, n):
            window.popleft()
            window.append(nums[i])
            if nums[i] > max_value:
                max_value = nums[i]
            #这里加这个判断是避免最大值在队列前面被弹出的情况
            if max_value not in window:
                max_value = max(window)
            res.append(max_value)
        return res
```

简单地说就是我维持了一个大小为k的双端队列，并且维持了一个最大值，随着窗口的移动看新加入的值是不是大于最大值。但是面临一个问题是如果最大值是窗口的最左端被弹出怎么办，这个时候我依旧没有好的方法，用的max函数来求，所以这个方法只是在暴力法的基础上稍微优化了一下，在最坏情况下还是上面的时间复杂度。

### 思路3 使用双端队列，并且双端队列的大小可以动态变化

使用双端队列作为数据结构，因为它保证了从两端以常数时间压入和弹出元素。

**一个关键的问题就是如果双端队列的大小是动态变化的，那么肯定就不是把窗口中的元素直接照搬进去，双端队列中存什么？因为输出的是滑动窗口中的最大值，所以我们可以想到把双端队列的按照数组元素的大小来依次存储，当我们滑动一次的时候，队列中所有比新加入的元素小的元素都应该弹出，因为它们肯定不是最大值。那么接下来思考滑动的时候，左端的情况，因为当队列的大小=k的时候，最左边的元素就应该弹出了，注意这里说的最左边的元素应该是nums中对应的滑动窗口中的最左边的元素，而此时队列中最左边的元素是最大值，并不一定需要弹出，所以这个时候一个关键点就出来了——我们应该存储的是对应元素的下边，当队列的长度为k并且队列的最前端存的是当前滑动窗口最左端的索引的时候，说明最大值就是滑动窗口的最左边的元素，我们就应该弹出了，而不是在队列中存数组中元素的值。**

```python
from collections import deque
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        if n * k == 0:
            return []
        if k == 1:
            return nums
        
        deq = deque()
        res = []
        max_value = 0
        #首先进行初始化
        for i in range(k):
            #如果deq不为空，并且当前队列的最前端存的索引已经不在滑动窗口里面了，我们就需要把最前面的元素进行pop
            if deq and deq[0] == i - k:
                deq.popleft()
            #当deq非空，我们对所有小于新滑进来的元素进行pop
            while deq and nums[i] > nums[deq[-1]]:
                deq.pop()
            #将索引添加进去
            deq.append(i)
        res.append(nums[deq[0]])
        #依次遍历，过程和上面差不多了
        for i in range(k, n):
            if deq and deq[0] == i - k:
                deq.popleft()
            while deq and nums[i] > nums[deq[-1]]:
                deq.pop()
            deq.append(i)
            res.append(nums[deq[0]])
        return res
```

**复杂度分析**

**时间复杂度：O(N)，每个元素被处理两次- 其索引被添加到双向队列中和被双向队列删除。**

**空间复杂度：O(N)，输出数组使用了 O(N−k+1) 空间，双向队列使用了 O(k)。**

