# 2020.12.8号 力扣第217题 存在重复元素

## 题目描述

给定一个整数数组，判断是否存在重复元素。如果任意一值在数组中出现至少两次，函数返回 `true` 。如果数组中每个元素都不相同，则返回 `false` 。

## 思路1

直接使用python的set()来进行去重然后比较前面元素个数是否发生变化。代码如下。

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        nums_set = set(nums)
        if len(nums) > len(nums_set):
            return True
        else:
            return False
```

## 思路2

使用sort之后查看列表是否存在相邻元素相同，其中python使用的sort函数的实现是Timesort，平均时间复杂度是O(nlogn),有兴趣的可以看下这篇博客。

https://www.cnblogs.com/clement-jiao/p/9243066.html