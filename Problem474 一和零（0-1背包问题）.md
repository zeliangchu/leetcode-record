# Problem474 一和零（0-1背包问题 416题同）

## 题目描述

给你一个二进制字符串数组strs和两个整数m和n，请你找出并返回strs的最大子集的大小，该子集中最多有m个0和n个1.

![image-20210101150813598](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210101150813598.png)

## 思路 还是0-1背包问题

https://leetcode-cn.com/problems/ones-and-zeroes/solution/dong-tai-gui-hua-0-1bei-bao-wen-ti-labuladongdong-/

直接看这个题解吧，讲的比较清楚。

这个题最开始我有点没弄明白的是dp数组代表的涵义是什么，其实很简单，题目问什么就把状态定义成什么。

代码如下：

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        if not strs:
            return 0
        l = len(strs)
        dp = [[[0 for _ in range(m + 1)] for _ in range(n + 1)] for _ in range(l+1)]

        for i in range(1, l+1):
            count0 = strs[i-1].count('0')
            count1 = strs[i-1].count('1')
            for j in range(n + 1):
                for k in range(m + 1):
                    if j >= count1 and k >= count0:
                        dp[i][j][k] = max(dp[i-1][j][k], dp[i-1][j-count1][k-count0] + 1)
                    #注意当当前字符串的0或者1的数量大于j或者k的时候，就将使用之前的状态
                    else:
                        dp[i][j][k] = dp[i-1][j][k]
        return dp[l][n][m]
```

然后优化空间差不多，也是和416题一个思路。