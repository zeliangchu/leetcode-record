# Problem617 合并二叉树

## 题目描述

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上的时候，两个二叉树的一些节点就会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则**不为** NULL 的节点将直接作为新二叉树的节点。

## 思路 递归

最开始我写的递归，比较麻烦。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1 and t2:
            return t2
        if t1 and not t2:
            return t1
        if not t1 and not t2:
            return t1
        self.helper(t1, t2)
        return t1

    def helper(self, t1, t2):
        if not t1 and not t2:
            return
        if t1 and not t2:
            return

        t1.val += t2.val

        if not t1.left and t2.left:
            t1.left = TreeNode(0)
            self.helper(t1.left, t2.left)
        else:
            self.helper(t1.left, t2.left)
        
        if not t1.right and t2.right:
            t1.right = TreeNode(0)
            self.helper(t1.right, t2.right)
        else:
            self.helper(t1.right, t2.right)

```

重新写一个：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque
class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1:
            return t2
        if not t2:
            return t1
        t1.val += t2.val
        t1.left = self.mergeTrees(t1.left, t2.left)
        t1.right = self.mergeTrees(t1.right, t2.right)
        return t1
```

