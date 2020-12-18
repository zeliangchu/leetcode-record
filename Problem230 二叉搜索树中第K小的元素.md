# Problem230 二叉搜索树中第K小的元素

## 题目描述

给定一个二叉搜索树，编写一个函数寻找其中第K小的元素。

## 思路 利用中序遍历进行寻找

代码如下：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        #思路 先进行中序遍历之后输出列表的第k个元素
        res = []
        self.InOrder(root, res)
        return res[k-1]

    def InOrder(self, root, res):
        if not root:
            return
        self.InOrder(root.left, res)
        res.append(root.val)
        self.InOrder(root.right, res)
```

**复杂度分析**

- 时间复杂度:O(n) n表示二叉搜索树的节点个数。
- 空间复杂度:O(n)

## 优化——剪枝

因为当我们找到中序遍历的第k个元素之后就不需要接着找了，所以可以进行剪枝。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    ans = 0
    count = 0
    def kthSmallest(self, root: TreeNode, k: int) -> int:
        #思路 先进行中序遍历之后输出列表的第k个元素
        res = []
        self.InOrder(root, k)
        return self.ans

    def InOrder(self, root, k):
        if not root:
            return
        self.InOrder(root.left, k)
        self.count += 1
        if self.count == k:
            self.ans = root.val
        if self.count > k:#剪枝
            return 
        self.InOrder(root.right, k)
```

**复杂度分析**

- 时间复杂度:O(k)
- 空间复杂度:取决于递归调用的深度

