# 剑指Offer10-1斐波那契数列

## 题目描述

写一个函数，输入n，求斐波那契数列的第n项。答案需要取模1e9+7

## 思路1 递归

```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        if n == 1:
            return 1
        return (self.fib(n - 1) + self.fib(n - 2)) % 1000000007
```

超时。

原因是大量的重复计算，因为求fib(n-1)的时候就需要求fib(n-2)，显然有大量的重复计算，可以加一个缓存lru_cache。

```python
from functools import lru_cache
class Solution:
    @lru_cache(None)
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        if n == 1:
            return 1
        return (self.fib(n - 1) + self.fib(n - 2)) % 1000000007
```

通过。

## 思路2 按照性质来写

```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        if n == 1:
            return 1
        count = 2
        pre1 = 0
        pre2 = 1
        while count <= n:
            res = pre1 + pre2
            pre1 =  pre2
            pre2 = res
            count += 1
        return res % 1000000007
```

## 思路3 动态规划

```python
from functools import lru_cache
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        if n == 1:
            return 1
        dp = [0] * (n + 1)
        dp[1] = 1
        for i in range(2, n + 1):
            dp[i] = dp[i-1] + dp[i-2]
        return dp[n] % 1000000007
```

优化空间,注意优化空间之后返回的是a，这里是需要想明白的。

```python
class Solution:
    def fib(self, n: int) -> int:
        a, b = 0, 1
        for _ in range(n):
            a, b =b, a+b
        return a % 1000000007
```

