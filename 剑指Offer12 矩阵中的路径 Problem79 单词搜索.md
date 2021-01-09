# 剑指Offer12 矩阵中的路径 Problem79 单词搜索

## 题目描述

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某个字符串所有字符路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

## 思路 dfs+回溯

直接上代码吧。

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        #使用dfs+回溯
        self.flag = False
        rows = len(board)
        if rows == 0:
            return False
        columns = len(board[0])
        if columns == 0:
            return False
        visited = set()
        directions = [(-1,0), (0,1), (1,0), (0,-1)]

        def backtracing(board, i, j, visited, idx):
            #剪枝 这一步的剪枝一定要有  要不然会超时
            if self.flag:
                return 
            #剪枝
            if i < 0 or i >= rows or j < 0 or j >= columns:
                return 
            #剪枝
            if word[idx] != board[i][j]:
                return
            if idx == len(word) - 1:
                self.flag = True
                return 
            for x, y in directions:
                if (i+x, j+y) not in visited:
                    #加入visited
                    visited.add((i+x, j+y))
                    #回溯
                    backtracing(board, i+x, j+y, visited, idx+1)
                    #退出visited
                    visited.remove((i+x, j+y))
    
        for i in range(rows):
            for j in range(columns):
                if self.flag:
                    return True
                if board[i][j] == word[0]:
                    visited.add((i, j))
                    backtracing(board, i, j, visited, 0)
                    visited.remove((i,j))
        return self.flag
```

上一个题解中的解法，这种写法没有回溯，但是挺简洁的。并且另一个巧妙的思想是没有使用visited，直接将字符串换成一个不存在的字符，用来标记已经访问过，后面再变回来。

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        def dfs(i, j, k):
            if not 0 <= i < len(board) or not 0 <= j < len(board[0]) or board[i][j] != word[k]: return False
            if k == len(word) - 1: return True
            board[i][j] = ''
            res = dfs(i + 1, j, k + 1) or dfs(i - 1, j, k + 1) or dfs(i, j + 1, k + 1) or dfs(i, j - 1, k + 1)
            board[i][j] = word[k]
            return res

        for i in range(len(board)):
            for j in range(len(board[0])):
                if dfs(i, j, 0): return True
        return False
```

