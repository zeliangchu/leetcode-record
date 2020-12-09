# 2020.12.8号 力扣第219题 存在重复元素2

## 题目描述

给定一个整数数组和一个整数，判断数组中是否存在两个不同的索引i和j，使得nums[i]=nums[j],并且i和j的差的绝对值至多为k。

## 思路1

我的第一种思路是分三步进行 1.找出重复元素 2.获得重复元素对应的下标列表 3.查看下标间隔是否满足要求。这个复杂度是O(n**2)，最后超时了。

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        #分三步进行 1.找出重复元素 2.获得重复元素对应的下标列表 3.查看下标间隔是否满足要求
        repeats = []#存放重复元素的数组
        for num in nums:
            if nums.count(num) > 1 and num not in repeats:
                repeats.append(num)
        if not repeats:
            return False
        for repeat in repeats:
            ndxs = [index for (index, number) in enumerate(nums) if number == repeat]#获取重复元素对应的下标列表
            for i in range(1, len(ndxs)):
                if ndxs[i] - ndxs[i-1] <= k:
                    return True
        return False
```

## 思路2

其实这个题我主要没有滑动窗口的概念，这个题应该维护一个大小为k的滑动窗口的。有了这个大小为k的滑动窗口然后再加散列表，就可以把复杂度降下来。

代码如下。

**时间复杂度：O(n) 因为set的查找、添加、删除操作都是常数时间内。**

**空间复杂度：O(min(n,k)) 空间取决于开的滑动窗口的大小。**

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        #建立一个大小不超过k的散列表或者说窗口
        k_windows = set()
        for i in range(len(nums)):
            #如果当前元素在窗口中存在了，说明满足条件了，直接返回true
            if nums[i] in k_windows:
                return True
            #否则将该元素添加到窗口中
            k_windows.add(nums[i])
            #检查窗口的大小是否超过了k，如果超过了k,意味着1.当前窗口一个重复元素都没有，因为如果有的话，肯定就返回true了 2.当前窗口的第一个元素也就是nums[i-k]没有用了，因为找不到它的在以k为大小的窗口内的重复元素，所以可以把它删除了。
            if len(k_windows) > k:
                k_windows.remove(nums[i-k])
        return False
```

