# 剑指Offer29 顺时针打印矩阵 Problem54螺旋矩阵

## 题目描述

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

## 思路 一层一层的向里遍历

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        result = []
        self.helper(matrix, result)
        return result

    def helper(self, matrix, result):
        if not matrix:
            return 
        #if len(matrix)
        if len(matrix) == 1:
            result += matrix[0]
            return
        rows = len(matrix)
        cols = len(matrix[0])
        for j in range(cols):
            result.append(matrix[0][j])
        for i in range(1, rows):
            result.append(matrix[i][cols-1])
        #注意这里是应对[[7],[8],[9]]这样的用例，这样的用例经过前面两次遍历就可以了，后面会导致重复
        if cols == 1 or rows == 1:
            return 
        for j in range(cols-2, -1, -1):
            result.append(matrix[rows-1][j])
        for i in range(rows-2, 0, -1):
            result.append(matrix[i][0])
        #注意这里是面对一次螺旋遍历之后正好遍历完 就是一圈之后里面没有矩阵了
        if cols == 2 or rows == 2:
            return 
        tmp = [[0 for _ in range(cols-2)] for _ in range(rows-2)]
        for i in range(rows-2):
            for j in range(cols-2):
                tmp[i][j] = matrix[i+1][j+1]
        self.helper(tmp, result)
```

## 思路2 分享评论区一个解法

- 首先设定上下左右边界
- 其次向右移动到最右，此时第一行因为已经使用过了，可以将其从图中删去，体现在代码中就是重新定义上边界
- 判断若重新定义后，上下边界交错，表明螺旋矩阵遍历结束，跳出循环，返回答案
- 若上下边界不交错，则遍历还未结束，接着向下向左向上移动，操作过程与第一，二步同理
- 不断循环以上步骤，直到某两条边界交错，跳出循环，返回答案

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        ans = []
        if not matrix:
            return ans
        #定义上下左右边界
        up, down, left, right = 0, len(matrix) - 1, 0, len(matrix[0]) - 1
        while True:
            for i in range(left, right + 1):
                ans.append(matrix[up][i])
            up += 1
            if up > down:
                break
            for i in range(up, down + 1):
                ans.append(matrix[i][right])
            right -= 1
            if right < left:
                break
            for i in range(right, left - 1, -1):
                ans.append(matrix[down][i])
            down -= 1
            if down < up:
                break
            for i in range(down, up - 1, -1):
                ans.append(matrix[i][left])
            left += 1
            if left > right:
                break
        return ans
```

