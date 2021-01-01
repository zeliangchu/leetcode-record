# Problem437 路径总和3

## 题目描述

![image-20210101153817508](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210101153817508.png)

## 思路 前序遍历一次递归＋从当前节点寻找路径一次递归

这个题唯一需要注意的是如果一个节点路径满足要求了，那需不需要向下找？答案是需要，因为可能存在下面的路径和为0，那么加上前面的路径就是一个新的路径。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    res = 0
    def pathSum(self, root: TreeNode, sum1: int) -> int:
        if not root:
            return 0
        self.Preorder(root, sum1)
        return self.res
    
    def Preorder(self, root, sum1):
        if not root:
            return 
        self.count(root, sum1)
        self.Preorder(root.left, sum1)
        self.Preorder(root.right, sum1)

    def count(self, root, target):
        if not root:
            return 
        if root.val == target:
            self.res += 1
        #这里一定要注意，如果当前节点已经满足root.val == target，它的根节点有可能能凑出和为0的路径，所以还要接着往下找
        self.count(root.left, target - root.val)
        self.count(root.right, target - root.val)
```

