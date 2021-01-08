# 剑指Offer07 重建二叉树

输入一个二叉树的前序遍历和中序遍历的结果，请重建该二叉树，假设输入的前序遍历和中序遍历的结果都不含有重复的数字

## 思路 递归

中心思想就是前序序列中的第一个元素在中序序列中的位置的左边就是左子树的中序序列，右边就是右子树的中序序列，递归构建就可以了。

唯一需要注意的就是在找到根节点在中序中的位置之后就将前序序列中第一个元素pop就可以了。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        #递归法
        if not preorder or not inorder:
            return 
        root = TreeNode(preorder[0])
        idx = inorder.index(preorder[0])
        preorder.pop(0)
        root.left = self.buildTree(preorder, inorder[:idx])
        root.right = self.buildTree(preorder, inorder[idx + 1:])
        return root
```

但是因为pop(0)的复杂度比较高，可以将preorder变成双端队列。