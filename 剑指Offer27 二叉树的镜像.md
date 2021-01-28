# 剑指Offer27 二叉树的镜像

## 题目描述

翻转二叉树

## 思路 自顶向下翻转

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        self.invertTree(root)
        return root
        
    def invertTree(self, root):
        if not root:
            return 
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
```

自低向下翻转就把互换的语句放到最后就可以了。