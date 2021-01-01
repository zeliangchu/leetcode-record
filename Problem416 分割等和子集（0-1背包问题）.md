# Problem416 分割等和子集（0-1背包问题）

## 题目描述

给定一个只包含正整数的非空数组，是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

## 思路 0-1背包问题

这个问题刚做起来可能感觉和背包问题没有什么关系。

我们首先看一下背包问题是什么？

- 给你一个可装载重量为w的书包和n个物品，每个物品有重量和价值两个属性，其中第i个物品的重量是wt[i] 价值为val[i] 现在让你用这个背包装物品，最多能装的价值是多少？

对于这个问题，我们可以先对集合求和，得到sum之后，这个问题就转换为下面这个问题：

- 给一个可以装载重量为sum/2的背包和n个物品，每个物品的重量是nums[i]，现在让你装背包，是否存在一种装法，恰好能将背包装满？

### 解题步骤

#### 1.明确状态和选择以及dp数组的定义

背包问题的状态就是背包的容量以及可选择的物品，选择就是装进背包还是不装进背包。

dp数组的涵义就是dp\[i][j]表示对于前i个物品，当前背包的容量为j的时候，如果dp\[i][j]为true，则说明可以从前i个物品中选出一种装法，可以恰好将背包装满，否则则说明不能恰好将背包装满。

#### 2.写状态转移方程

如果不把nums[i]算入子集中，或者说你不把第i个物品放入书包中，那么是否能够装满书包取决于上一个状态dp\[i-1][j]

如果把nums[i]算入子集中，或者说把第i个物品放入书包中，那么是否能够装满书包取决于dp\[i-1][j-nums[i-1]]

注意之所以是i-1是因为i是从1开始的，但是数组索引是从0开始的。

所以代码如下：

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if not nums:
            return True
        n = len(nums)
        s = sum(nums)
        if s % 2 != 0:
            return False
        target = s // 2
        dp = [[False for _ in range(target+1)] for _ in range(n + 1)]

        for i in range(n + 1):
            dp[i][0] = True

        for i in range(1, n+1):
            for j in range(1, target + 1):
                if j - nums[i - 1] < 0:
                    dp[i][j] = dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j] or dp[i-1][j-nums[i - 1]]
        
        return dp[n][target]
```

我们发现dp\[i][j]都是通过上一行dp\[i-1][j]转移过来的，所以可以进行空间优化，优化后的代码如下：

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if not nums:
            return True
        n = len(nums)
        s = sum(nums)
        if s % 2 != 0:
            return False
        target = s // 2
        dp = [False for _ in range(target+1)]

        dp[0] = True

        for i in range(1, n+1):
            for j in range(target, 0, -1):
                if j - nums[i - 1] >= 0:
                    dp[j] = dp[j] or dp[j-nums[i - 1]]
        
        return dp[target]
```

这里有一个很关键的点是j是逆序遍历的，解释如下：

![image-20210101135233204](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210101135233204.png)

## 链接——474 494 879（0-1背包问题） 322 518 1449（完全背包问题）

![image-20210101135356059](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210101135356059.png)