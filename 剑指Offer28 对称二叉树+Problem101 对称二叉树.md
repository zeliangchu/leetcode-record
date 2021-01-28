# 剑指Offer28 对称二叉树+Problem101 对称二叉树

## 题目描述

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

## 思路1

按照题目中说的，将二叉树和它的镜像进行比较。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        tmp = self.copyTree(root)
        self.invertTree(root)
        return self.isSameTree(tmp, root)

    def invertTree(self, root):
        if not root:
            return 
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)

    def copyTree(self, root):
        if not root:
            return None
        new_root = TreeNode(root.val)
        new_root.left = self.copyTree(root.left)
        new_root.right = self.copyTree(root.right)
        return new_root
    
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q:
            return True
        if not p or not q:
            return False
        if p.val == q.val:
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        else:
            return False
```

**复杂度分析：**

- 时间复杂度：O(3N) N表示二叉树节点的个数，其中深拷贝二叉树、翻转二叉树和判断二叉树是否相同都需要O(N)的时间。
- 空间复杂度：O(3N) 最坏情况下，三个递归使用的栈的深度都是O(N)

## 思路2 直接将左右子树进行比较

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        return self.helper(root.left, root.right)
    
    def helper(self, root1, root2):
        if not root1 and not root2:
            return True
        if not root1 or not root2:
            return False
        return root1.val == root2.val and self.helper(root1.left, root2.right) and self.helper(root1.right, root2.left)
```

**复杂度分析：**

- 时间复杂度：O(N) N表示二叉树节点的个数
- 空间复杂度：O(N)