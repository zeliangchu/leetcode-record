# Problem543 二叉树的直径

## 题目描述

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个节点路径长度中的最大值，这条路径可能穿过也可能不穿过根节点。

## 思路1 我自己的思路

二叉树的直径其实就是求一个二叉树的所有叶子节点之间的路径长度的最大值，或者这么说，求所有节点的左子树的深度加右子树的深度的最大值。

基于此思路我才用前序遍历来找所有节点的左子树深度加右子树深度的最大值。

代码如下：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    res = 0
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        if not root:
            return 0
        self.PreOrder(root)
        return self.res
    
    def PreOrder(self, root):
        if not root:
            return 
        if self.maxDepth(root.left) + self.maxDepth(root.right) > self.res:
            self.res = self.maxDepth(root.left) + self.maxDepth(root.right)
        self.PreOrder(root.left)
        self.PreOrder(root.right)

    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        queue = [root]
        res = 0
        while queue:
            tmp = []
            for node in queue:
                if node.left:
                    tmp.append(node.left)
                if node.right:
                    tmp.append(node.right)
            queue = tmp
            res += 1
        return res
```

## 思路2 深度优先搜索

一条路径的长度即为该路径经过的节点上数-1，所以求直径等价于求路径经过的节点数-1.

而任意一条路径都可以看作由一个节点为起点，从其左儿子和右儿子向下遍历的路径拼接得到的。

对于二叉树中的某个节点，假设知道了该节点的左二子向下遍历经过的最多节点数L（其实也就是以左二子为根的子树的深度）和右儿子向下遍历经过的最多的节点数R，那么经过该节点的所有路径中经过节点数最多的路径，它经过的节点数为L+R+1.

那么我们对二叉树中所有节点求它的这样一个L+R+1,那么二叉树的直径就是前面求的所有值中的最大值-1.

其实下面的代码就是求二叉树深度的递归版本加了一个全局变量，并更新的过程。

```
class Solution(object):
    def diameterOfBinaryTree(self, root):
        self.ans = 1
        def depth(node):
            # 访问到空节点了，返回0
            if not node:
                return 0
            # 左儿子为根的子树的深度
            L = depth(node.left)
            # 右儿子为根的子树的深度
            R = depth(node.right)
            # 计算d_node即L+R+1 并更新ans
            self.ans = max(self.ans, L+R+1)
            # 返回该节点为根的子树的深度
            return max(L, R) + 1

        depth(root)
        return self.ans - 1
```

**复杂度分析**

时间复杂度：O(N)，其中 NN 为二叉树的节点数，即遍历一棵二叉树的时间复杂度，每个结点只被访问一次。

空间复杂度：O(Height)，其中 HeightHeight 为二叉树的高度。由于递归函数在递归过程中需要为每一层递归函数分配栈空间，所以这里需要额外的空间且该空间取决于递归的深度，而递归的深度显然为二叉树的高度，并且每次递归调用的函数里又只用了常数个变量，所以所需空间复杂度为 O(Height) 。
