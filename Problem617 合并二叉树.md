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

## 思路2 广度优先遍历

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1:
            return t2
        if not t2:
            return t1
        
        merged = TreeNode(t1.val + t2.val)
        queue = collections.deque([merged])
        queue1 = collections.deque([t1])
        queue2 = collections.deque([t2])

        while queue1 and queue2:
            node = queue.popleft()
            node1 = queue1.popleft()
            node2 = queue2.popleft()
            left1, right1 = node1.left, node1.right
            left2, right2 = node2.left, node2.right
            if left1 or left2:
                if left1 and left2:
                    left = TreeNode(left1.val + left2.val)
                    node.left = left
                    queue.append(left)
                    queue1.append(left1)
                    queue2.append(left2)
                elif left1:
                    node.left = left1
                elif left2:
                    node.left = left2
            if right1 or right2:
                if right1 and right2:
                    right = TreeNode(right1.val + right2.val)
                    node.right = right
                    queue.append(right)
                    queue1.append(right1)
                    queue2.append(right2)
                elif right1:
                    node.right = right1
                elif right2:
                    node.right = right2
        
        return merged
```

