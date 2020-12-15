# problem229 求众数2

## 题目描述

给定一个大小为n的整数数组，找出其中所有出现超过⌊ n/3 ⌋次的元素。

请尝试设计出时间复杂度为O(n)、空间复杂度为O(1)的算法。

## 思路1 使用字典计数

使用字典计数。

代码如下：

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:
        if not nums:
            return []
        n = len(nums)
        if n == 1:
            return nums
        dic = {}
        for i in nums:
            if i not in dic:
                dic[i] = 1
            else:
                dic[i] += 1
        return [key for key in dic if dic[key] > n // 3 ]
```

复杂度分析：

- 时间复杂度：O(n)
- 空间复杂度：最坏情况下就是所有元素都出现一次，这时候dic的空间复杂度就是O(n)

## 思路2 摩尔投票法

摩尔投票法是用来解决如何在任意多的候选人中选出票数超过一半的那个人。**摩尔投票法的算法思路是设置一个候选人和一个计数器count,遍历数组，每次遇到和候选人一样的数字就将count+1，否则就将count-1，当count=0的时候替换候选人。这个过程结束之后就得到要求的众数。**

那么这个题变成了找到个数大于长度的1/3的数字，一个很显然的事情是满足这个条件的数字不会超过两个。那么我们这个时候算法的思想就是设置两个候选人以及计数器，当遍历到和候选人相同的数字之后就将计数器加一，如果和两个候选人的数字都不相同，就将两个候选人的计数器都减一。当一个计数器为0的时候，就更换对应的候选人。但是这个问题并不能保证一定会有两个候选人，有可能到最后才出现一个元素D，这个元素之前从来没有出现过，而且两个候选人中的某一个正好到这里count=0，所以D这个候选人就会出现遍历完count=1的情况，所以我们还需要再进行一次遍历来检查最后的两个候选人是否满足题目条件。

代码如下：

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:
        if not nums:
            return []
        num1 = num2 = nums[0]
        time1 = time2 = 0
        for num in nums:
            if num1 == num:
                time1 += 1
                continue
            if num2 == num:
                time2 += 1
                continue
            if time1 == 0:
                num1 = num
                time1 += 1
                continue
            if time2 == 0:
                num2 = num
                time2 += 1
                continue
            time1 -= 1
            time2 -= 1
        time1 = time2 = 0
        #print(num1,num2)
        for num in nums:
            if num == num1:
                time1 += 1
            elif num == num2:
                time2 += 1
        res = []
        n = len(nums)
        #print(time1, time2)
        if time1 > n/3:
            res.append(num1)
        if time2 > n/3:
            res.append(num2)
        return res
```

