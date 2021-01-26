# 剑指Offer14-1 剪绳子（Problem342-整数拆分）

## 题目描述

给定一个正整数 *n*，将其拆分为**至少**两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

## 思路1 动态规划

动态规划的思路还是比较好想的。

```python
class Solution:
    def cuttingRope(self, n: int) -> int:
        dp = [1 for _ in range(n+1)]
        dp[1] = 1
        dp[2] = 1
        for i in range(2, n + 1):
            for j in range(1, i):
                tmp = max(j * (i - j), dp[j] * (i - j), dp[j] * dp[i - j])
                if tmp > dp[i]:
                    dp[i] = tmp
        return dp[n]
```

但是我这里max之中传入了三个参数，其实只需要传入两个。

当i>=2的时候，假设正整数i拆分出来的第一个正整数是j（1<=j<i)，则有以下两种方案：

- 将i拆分成j和i-j的和，且i-j不再拆分成多个正整数，此时的乘积为j*(i-j)
- 将i拆分为j和i-j的和，且i-j继续拆分为多个正整数，此时的乘积是j*dp[i-j]

所以代码可以简化为：

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [0] * (n + 1)
        for i in range(2, n + 1):
            for j in range(i):
                dp[i] = max(dp[i], j * (i - j), j * dp[i - j])
        return dp[n]
```

**复杂度分析：**

- 时间复杂度：O(n^2)
- 空间复杂度：O(n)

## 思路2 数学方法

主要根据算术几何均值不等式，然后利用求极大值的一些东西。

这个题解写的挺不错的。

https://leetcode-cn.com/problems/integer-break/solution/343-zheng-shu-chai-fen-tan-xin-by-jyd/

主要是那个拆分规则。

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        if n < 4:
            return n - 1
        a, b = divmod(n , 3)
        if b == 0:
            return pow(3, a)
        if b == 1:
            return pow(3, a - 1) * 4
        return pow(3, a) * 2
```

时间复杂度和空间复杂度都是常数级别的。