# Problem240 搜索二维矩阵2

## 题目描述

编写一个高效的算法来搜索m*n矩阵matrix中的一个目标值，该矩阵具有以下特性：

- 每行的元素由左到右升序排列
- 每列的元素从上到下升序排列

## 思路1 暴力法 但是可以稍微优化一下

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        n = len(matrix[0])
        if m == 0 or n == 0:
            return False
        for i in range(m):
            for j in range(n):
                if j == 0 and matrix[i][j] > target:
                    return False
                if matrix[i][j] == target:
                    return True
                if matrix[i][j] > target:
                    break
```

最坏情况下的时间复杂度还是O(m*n),空间复杂度是常数级别的。

## 思路2 深度优先搜索

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        n = len(matrix[0])
        if m == 0 or n == 0:
            return False
        
        def dfs(matrix, i, j, target):
            if i == m or j == n or matrix[i][j] == '#':
                return False
            if matrix[i][j] == target:
                return True
            elif matrix[i][j] > target:
                return False
            else:
                matrix[i][j] = '#' #标记为已访问
                return dfs(matrix, i + 1, j, target) or dfs(matrix, i, j + 1, target)
        
        return dfs(matrix, 0, 0, target)
```

这个方法并没有做到更好，因为在最坏情况下已经是需要遍历整个数组，也就是时间复杂度是O(m*n)，反而空间复杂度因为递归的使用导致了空间复杂度增加。

## 思路3 对每一行进行二分查找

这个方法在思路一的基础上进行二分查找，从而将每一行的查找时间缩短为logn

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        n = len(matrix[0])
        if m == 0 or n == 0:
            return False
        for i in range(m):
            if matrix[i][0] > target:
                return False
            l, r = 0, n - 1
            while l <= r:
                mid = l + r >> 1
                if matrix[i][mid] == target:
                    return True
                elif matrix[i][mid] > target:
                    r = mid - 1
                else:
                    l = mid + 1
        return False
```

**复杂度分析：**

- 时间复杂度：O(mlogn)
- 空间复杂度：O(1)

## 思路4 最巧妙的解法

- 左下角的元素是这一行中的最小的元素，同时又是这一列中最大的元素。比较左下角元素和目标。
  - 如果左下角的元素等于目标，返回true
  - 如果左下角的元素大于目标，也就是说这一行中的元素都大于目标，问题规模就可以减少为在去掉最后一行的子矩阵中寻找目标
  - 如果左下角的元素小于目标，也就是说这一列的元素都小于目标，问题规模就可以减少为在去掉第一列的子矩阵中寻找目标
- 如果最后矩阵减小为空，则说明不存在

这个解法的一个关键点是为什么选择左下角的元素，其实可以这样解释。

- 选左上角，往右走和往下走都增大，不能选
- 选右下角，往上走和往左走都减小，不能选
- 选左下角，往右走增大，往上走减小，可选
- 选右上角，往下走增大，往左走减小，可选

**这个二维数组其实就类似一棵排序二叉树，对于每一个数来说，上方的数都小于它，右边的数都大于它，所以把左下角作为根节点开始查找。**

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix)
        n = len(matrix[0])
        if m == 0 or n == 0:
            return False
        #从左下角开始
        i = m - 1
        j = 0
        while i >= 0 and j < n:
            if matrix[i][j] == target:
                return True
            elif matrix[i][j] < target:
                j += 1
            else:
                i -= 1
        return False
```

**复杂度分析：**

- 时间复杂度：O(m+n)
- 空间复杂度：O(1)

