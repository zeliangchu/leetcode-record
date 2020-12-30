# Problem347 前k个高频元素

## 题目描述

给定一个非空整数数组，返回其中出现频率前k高的元素。

## 思路1 使用Counter模块

![image-20201231003534595](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201231003534595.png)

```python
from collections import Counter
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        return [_[0] for _ in Counter(nums).most_common(k)]
```

## 思路2 时间复杂度为O(n)的解法

这种方法的关键在于构造res这个二维数组，然后数组的第一维度代表出现的次数，所有下标从高到底对应出现次数的高低。这个解法真的非常棒了。

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        nums_time_dict = {}
        res = [[] for i in range(len(nums)+1)]
        for i in nums:
            if i in nums_time_dict:
                nums_time_dict[i] += 1
            else:
                nums_time_dict[i] = 1
        
        for num, times in nums_time_dict.items():
            res[times].append(num)
        
        ans = []
        for i in range(len(nums), 0, -1):
            if len(res[i]) == 0:
                continue
            ans.extend(res[i])
            if len(ans) == k:
                return ans
```

### 思路3 使用堆

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dic = collections.Counter(nums)
        heap, ans = [], []
        for i in dic:
            heapq.heappush(heap, (-dic[i], i))
        for _ in range(k):
            ans.append(heapq.heappop(heap)[1])
        return ans
```

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        from collections import Counter
        import heapq
        count = Counter(nums)   
        return heapq.nlargest(k, count.keys(), key=count.get)
```

```python
# 优先队列；维护大小为k的堆 -- 时间复杂度O(nlogk) 当k比较小时合适
# 优先队列；n + nlogk， 时间复杂度： O(nlogk)
# 下面只从堆的角度考虑，从m个元素中，通过堆选出最大的k个数
# k大小的小根堆；堆满后，若新加的数大于堆首数，弹出堆首元素 -- 弹出了m-k个最小的
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        from collections import Counter
        import heapq as hq
        lookup = Counter(nums)
        res = []
        heap = []
        for num, freq in lookup.items() :
            # 如果堆满了（k个元素）
            if len(heap) == k :
                # 弹出最小频率的元组
                if heap[0][0] < freq:
                    hq.heapreplace(heap, (freq, num))
            else : 
                hq.heappush(heap, (freq, num))
        while heap :
            res.append(hq.heappop(heap)[1])
        
        return res
```

```python
# 优先队列；维护大小为n-k的堆 -- 时间复杂度O(nlog(n-k)) 当k比较大时合适
# 时间复杂度：O(nlog(n-k))；当k和n相当时，用此法
# 从m个元素中，通过堆选出最大的k个数；m-k 大小的堆 -- 大根堆
# 堆满后，若新加的数小于堆首数，堆首元素加入结果，否则新加的数加入结果 -- 选了k次最大的
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        from collections import Counter
        import heapq as hq
        res = []

        heap = []
        lookup = Counter(nums)
        n = len(lookup)
        for num, freq in lookup.items() :
            if len(heap) == n-k :
                if heap and -heap[0][0] > freq :
                    res.append(hq.heapreplace(heap, (-freq, num))[1])
                else : res.append(num)
            else :
                hq.heappush(heap, (-freq, num))
        return res
```

## 思路4 快速排序

https://leetcode-cn.com/problems/top-k-frequent-elements/solution/leetcode347onfu-za-du-bu-fen-si-xiang-ji-dui-jie-f/

可以参看一下这个题解

## 链接 Problem215 数组中第k个最大的元素

https://leetcode-cn.com/problems/kth-largest-element-in-an-array/