# 2020.12.13 力扣第226题 翻转二叉树

## 题目描述

翻转一棵二叉树

## 思路一 递归

递归也可以有两种写法，分别是从上往下交换还是从下往上交换。

我最开始写的是自上往下。

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        def helper(root):
            if root is None:
                return
            root.left, root.right = root.right, root.left
            helper(root.left)
            helper(root.right)
        
        helper(root)
        return root
```

复杂度分析：

- 时间复杂度：O(n) 其中n表示节点的个数
- 空间复杂度：空间复杂度取决于递归调用栈的高度，平均是O(logn) 最坏情况是O(n) 当二叉树是一条链的形状的时候最慢  

自下往上的代码

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        root.left, root.right = right, left
        return root
```

