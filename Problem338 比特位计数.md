# Problem338 比特位计数

## 题目描述

给定一个非负整数num,对于0<=i<=num范围中的每个数字i，计算其二进制数中的1的数目并将它们作为数组返回。

## 思路1 暴力法

暴力法的思路很简单，直接看代码吧。

```python
class Solution:
    def countBits(self, num: int) -> List[int]:
        res = []
        for i in range(num + 1):
            res.append(bin(i).count('1'))
        return res
```

## 思路2 动态规划

接下来我给大家分析一下如何根据题目中给的小提示一步步的得到复杂度O(n)的解法。

先给大家放一张图来看一下我观察num=15的时候输出数组的规律，你发现了规律之后就明白了一大半了。

![image-20201229231954145](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201229231954145.png)

我为什么会想到图中的分隔方法呢？

![image-20201229231204207](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201229231204207.png)

因为题目有提示，我根据提示把num=15的输出的数组下标划分了一下。发现的规律就是我图中不同的组对比的规律，其实有了图中规律就很好写代码了，用一个power变量来方便表示当前区间的范围以及下标之间的对应关系应该减几。

代码如下：

```python
class Solution:
    def countBits(self, num: int) -> List[int]:
        if num == 0:
            return [0]
        if num == 1:
            return [0,1]
        dp = [0 for _ in range(num+1)]
        dp[1] = 1
        power = 1
        for i in range(2, num + 1):
            dp[i] = dp[i - 2 ** power] + 1
            if i == 2 ** (power + 1) - 1:
                power += 1
        return dp
```

