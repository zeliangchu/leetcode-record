# 2020.12.10号 力扣第221题 最大正方形

## 题目描述

在一个由‘0’和‘1’组成的二维矩阵中，找到只包含'1'的最大正方形，并返回其面积。

## 思路一 暴力法

暴力法思路：

1. 遍历矩阵中的每一个元素，如果遇到1，就将该元素作为正方形的左上角
2. 根据左上角的行和列计算可能的最大的正方形的边长，然后在该范围内寻找只包含1的最大正方形
3. 每次在下方新增一行以及在右方新增一列，判断新增的行和列是否满足所有元素都是1

代码如下：

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        maxSide = 0
        rows, columns = len(matrix), len(matrix[0])
        for i in range(rows):
            for j in range(columns):
                if matrix[i][j] == '1':
                    maxSide = max(maxSide, 1)#这里主要是为了更新前面设置的maxSize = 0
                    curMaxSide = min(rows - i, columns - j)
                    for k in range(1, curMaxSide):
                        #实现使用一个flag来作为标记，只要出现flag=False的情况就break
                        flag = True
                        #判断新增的一行一列是否均为1
                        for m in range(k+1):
                            if matrix[i+k][j+m] == '0' or matrix[i+m][j+k] == '0':
                                flag = False
                                break
                        #如果新增的一行一列均为1，更新最大边长
                        if flag:
                            maxSide = max(maxSide, k + 1)
                        #否则退出，这里相当于用两个break退出了一个双重循环
                        else:
                            break
        
        maxSquare = maxSide ** 2
        return maxSquare
```

**复杂度分析：**

- 时间复杂度：O(mnmin(mn)**2)，其中m,n是矩阵的行数和列数
- 空间复杂度：O(1)

## 思路二 动态规划

### 1.明确dp(i,j)的含义

对于动态规划，我们首先要想明白dp\[i]\[j]表示的含义，因为动态规划是通过前面的状态推导后面的状态，也就是说dp\[i]\[j]是和前面状态相关的，所以我们用它来表示以(i,j)为右下角，且只包含1的正方形的边长的最大值。然后计算所有的dp(i,j)的值，最后这些值中的最大值的平方就是最后的答案。

### 2.转移方程的推导

那么接下来就要转移方程的推导。对于每个位置(i,j)，检查在矩阵中该位置的值：

- 如果该位置的值是0，那么dp(i,j)=0

- 如果该位置的值是1，那么dp(i,j)的值由它的上方，左边和左上方的三个位置的dp决定。具体而言就是下面这个方程：

  ```
  dp(i,j) = min(dp(i-1,j), dp(i-1,j-1), dp(i,j-1))+1
  ```

  这个方程的推导在https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/solution/tong-ji-quan-wei-1-de-zheng-fang-xing-zi-ju-zhen-2/这个链接里，这里的推导的主要思想是固定dp(i,j)来判断其他三个方向的dp和当前位置的关系，然后反过来固定其他三个方向的dp来判断dp(i,j)和其他三个方向的关系。正向推导，反向推导。

### 3.边界条件

如果i或者j中至少有一个为0，而且该位置的值为'1'，那么以该位置为右下角的最大正方形的边长只能是1。

代码如下：

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        maxSide = 0
        rows, columns = len(matrix), len(matrix[0])
        dp = [[0] * columns for _ in range(rows)]
        for i in range(rows):
            for j in range(columns):
                if matrix[i][j] == '1':
                    if i == 0 or j == 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
                    maxSide = max(maxSide, dp[i][j])
        
        maxSquare = maxSide ** 2
        return maxSquare
```

**复杂度分析：**

- 时间复杂度：O(m*n)
- 空间复杂度：O(m*n)

## 思路三 空间优化的动态规划解法

在上面的动态规划中，我们在计算第i行的时候我们其实只用到了当前元素的前一个元素以及第i-1行，所以我们其实可以将二维数组优化为一维数组。

最开始我们将一维数组全部初始化为0，然后我们一行一行的扫描矩阵，按照如下状态转移公式来更新当前的dp[j]:

```
dp[j] = min(prev,dp[j-1],dp[j])
```

分别解释一下min中的参数，第一个参数prev表示是之前的dp[j-1],第二个参数dp[j-1]就是现在的dp[j-1],然后第三个参数就是之前的dp[j]，然后通过比较它们的最小值得到当前的dp[j]。

|    prev     |     dp[j]     |
| :---------: | :-----------: |
| **dp[j-1]** | **new_dp[j]** |

最终的代码如下。

```python
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        maxSide = 0
        rows, columns = len(matrix), len(matrix[0])
        dp = [0 for _ in range(columns)]
        for i in range(rows):
            for j in range(columns):
                tmp = dp[j]#使用tmp来保存prev
                '''
                这里需要好好思考的一个问题是为什么的dp[j]（注意这里的dp[j]是老的dp[j]，新的还没有算，还没有更新）就变成了tmp(prev，后面将tmp赋值为prev)
                从dp[j]到prev，我们看上面的表格，其实相当于向左移动一步，这里的移动就是因为循环造成的，因为你循环一次之后原来的dp[j]是不是就变成了dp[j-1]也就是prev
                '''
                if matrix[i][j] == '1':
                    if i == 0 or j == 0:
                        dp[j] = 1
                    else:
                        dp[j] = min(prev, dp[j-1], dp[j]) + 1
                    maxSide = max(maxSide, dp[j])
                else:
                    dp[j] = 0
                #将tmp赋给prev
                prev = tmp
        
        maxSquare = maxSide ** 2
        return maxSquare
```

**复杂度分析：**

- 时间复杂度：O(mn)
- 空间复杂度：O(n)

## 链接——第1277题https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/

附上127题的代码如下：

二维数组

```python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        #学习了221题之后做出来的
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        rows = len(matrix)
        columns = len(matrix[0])
        dp = [[0] * columns for _ in range(rows)]
        res = 0
        for i in range(rows):
            for j in range(columns):
                if matrix[i][j]:
                    if i == 0 or j == 0:
                        dp[i][j] = 1
                        res += dp[i][j]
                    else:
                        dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1
                        res += dp[i][j]
        return res
```

一维数组

```python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        #学习了221题之后做出来的
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        rows = len(matrix)
        columns = len(matrix[0])
        dp = [0 for _ in range(columns)]
        res = 0
        for i in range(rows):
            for j in range(columns):
                tmp = dp[j]
                if matrix[i][j]:
                    if i == 0 or j == 0:
                        dp[j] = 1
                        res += dp[j]
                    else:
                        dp[j] = min(prev, dp[j-1], dp[j]) + 1
                        res += dp[j]
                else:
                    dp[j] = 0
                prev = tmp
        return res
```

