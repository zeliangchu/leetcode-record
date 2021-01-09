# 剑指Offer13 机器人的运动范围

## 题目描述

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

## 思路 dfs

这个题应该注意的是不能直接二维扫描然后判断每个i j是否满足要求，因为可能面临i j满足要求但是机器人从坐标(0,0)过不去的情况，所以这个题还是应该用类似于矩阵搜索的方法。

直接上代码：

```python
class Solution:
    def movingCount(self, m: int, n: int, k: int) -> int:
        count = 0
        
        matrix = [[0 for _ in range(n)] for _ in range(m)]

        def dfs(i, j):
            if not 0 <= i < m or not 0 <= j < n or matrix[i][j] == '' or (self.getSum(i) + self.getSum(j) > k):
                return
            matrix[i][j] = ''
            dfs(i+1, j)
            dfs(i-1, j)
            dfs(i, j+1)
            dfs(i, j-1)
        
        dfs(0,0)

        for i in range(m):
            for j in range(n):
                if matrix[i][j] == '':
                    count += 1
        return count
    #求数位之和的函数
    def getSum(self, number):
        res = 0
        a, b = divmod(number, 10)
        res += b
        while a:
            a, b = divmod(a, 10)
            res += b
        return res
```

时间复杂度和空间复杂度都是O(mn)