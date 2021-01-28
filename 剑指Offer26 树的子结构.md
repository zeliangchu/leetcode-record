# 剑指Offer26 树的子结构

## 题目描述

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

## 思路 使用前序遍历遍历A树，当出现结点的值相等就检查

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    flag = False
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if not A or not B:
            return False
        self.PreOrder(A, B)
        return self.flag

    def PreOrder(self, A, B):
        if not A:
            return 
        #检查
        if A.val == B.val:
            self.flag = self.check(A, B)
        self.isSubStructure(A.left, B)
        self.isSubStructure(A.right, B)

    def check(self, A, B):
        #做这种检查的时候先把空写上去
        if not A and not B:
            return True
        if not A and B:
            return False
        if not B and A:
            return True
        if A.val != B.val:
            return False
        return self.check(A.left, B.left) and self.check(A.right, B.right)
```

**复杂度分析：**

- 时间复杂度：O(MN)，其中M N分别为树A和树B的节点数量
- 空间复杂度：O(M)，当两棵树都退化为链表的时候，递归调用深度最大

一种简洁的写法：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def isSubStructure(self, A: TreeNode, B: TreeNode) -> bool:
        if not A or not B:
            return False
        return self.dfs(A, B) or self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B)
    
    def dfs(self, A, B):
        if not B:
            return True
        if not A:
            return False
        return A.val == B.val and self.dfs(A.left, B.left) and self.dfs(A.right, B.right)
```

其实这种写法的dfs就相当于是前序遍历中的print操作。