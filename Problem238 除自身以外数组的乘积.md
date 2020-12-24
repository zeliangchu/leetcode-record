# Problem238 除自身以外数组的乘积

## 题目描述

给定一个长度为n的整数数组nums,其中n>1,返回数组output，其中output[i]等于nums中除了当前位置之外其他元素的乘积。

说明：此题不能使用除法，并且在O(n)的时间复杂度内完成此题。

## 思路

这个题最开始我是不会的，其实这个题的结果就是当前位置的左边所有的数乘上右边所有的数，但是这个题的关键是通过两次遍历，第一次遍历将数组中所有的数的左边数字的乘积保存下来，第二次反向遍历将数组中所有的数的左边的乘积（在前一次遍历中已经计算出来）乘上右边所有的数的乘积。

最终代码如下：

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        #当前位置的结果等于它左边所有数的乘积乘上右边所有数的乘积
        left, right = 1, 1
        n = len(nums)
        res = [0 for _ in range(n)]
        for i in range(n):
            res[i] = left
            left *= nums[i]
        for j in range(n - 1, -1, -1):
            res[j] *= right
            right *= nums[j]
        return res
```

时间复杂度满足要求，空间复杂度不算最后结果的输出的话是常数空间复杂度。