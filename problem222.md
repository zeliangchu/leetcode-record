# 2020.12.11号 力扣第222题 完全二叉树的节点个数

## 题目描述

给出一个完全二叉树，求出该树的结点个数。

完全二叉树的定义如下：在完全二叉树中除了最底层节点没有填满外，其它每层节点数达到最大值，并且最底下一层的若干节点都集中在该层最左边的若干位置。

## 思路一 使用递归计算节点个数

代码如下：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        left_count = self.countNodes(root.left)
        right_count = self.countNodes(root.right)
        return left_count + right_count + 1
```

**复杂度分析：**

- 时间复杂度：O(n) 其中n表示二叉树中的节点个数
- 空间复杂度：O(logn) 

## 思路二 利用完全二叉树的性质

对于满二叉树来说，如果满二叉树的层数为h，那么总节点数是2^h-1

那么我们对root节点的左右子树进行高度统计，分别记为left和right，那么有以下两种情况：

1. left=right，这说明左子树一定是满二叉树。所以左子树的节点总数就是2^left-1，再加上root节点，正好是2^left，然后再对右子树进行递归处理。
2. left!=right，这说明最后一层不满，但是倒数第二层已经满了，也就是右子树一定是满二叉树，右子树加上root的节点个数是2^right.然后我们只需要对左子树进行递归检查。

有一个需要注意的点就是计算2^left的最快方法就是移位计算，记得因为运算符的优先级的问题加括号。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def countNodes(self, root: TreeNode) -> int:
        def countDepth(root):
            depth = 0
            while root:
                depth += 1
                root = root.left
            return depth

        if not root:
            return 0
        left_depth = countDepth(root.left)
        right_depth = countDepth(root.right)
        if left_depth == right_depth:
            return self.countNodes(root.right) + (1 << left_depth)
        else:
            return self.countNodes(root.left) + (1 << right_depth)
```

## 思路三 二分查找+位运算

二分查找+位运算暂时还没有搞明白。