# 剑指Offer03 数组中重复的数字

## 题目描述

找出数组中重复的数字，在一个长度为n的数组nums里的所有数字都在0到n-1之间，数组中的某些数字事重复的，但是不知道哪几个数字重复了，也不知道每个数字重复了几次，请找出数组中任意一个重复的数字。

## 思路1 使用哈希表

```python
class Solution:
    def findRepeatNumber(self, nums: List[int]) -> int:
        dic = dict()
        for i in range(len(nums)):
            if nums[i] not in dic:
                dic[nums[i]] = 1
            else:
                return nums[i]
```

时间和空间复杂度是O(n) 

## 思路2 

我一开始想的是，这个思路其实就是Problem287一样的题，构造一个环形链表，然后使用快慢指针找到入口元素。

但是其实是不对的，因为这个题的主要的0到n-1，287题的条件是：

给定一个包含n+1个整数的数组nums，其数字都在1-n之间（包括1和n），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

所以这个思路不对。

## 思路3 原地置换

看这个题解的思路就差不多明白了。

https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/solution/mian-shi-ti-03-shu-zu-zhong-zhong-fu-de-shu-zi-yua/

![image-20210108180544676](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210108180544676.png)

```python
class Solution:
    def findRepeatNumber(self, nums: [int]) -> int:
        i = 0
        while i < len(nums):
            if nums[i] == i:
                i += 1
                continue
            if nums[nums[i]] == nums[i]: return nums[i]
            nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
        return -1
```

