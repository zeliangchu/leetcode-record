# Problem236 二叉树的最近公共祖先

## 题目描述

给定一个二叉树，找出该树种两个指定节点的最近公共祖先。注意一个节点也可以是它自己的祖先。

## 思路1 递归

这个题思路就是如果两个节点位于不同子树中，那么直接就返回root,如果两个节点位于相同的子树中，就进入这个子树递归调用该函数。那么如何判断两个节点都位于哪个子树呢，我这里采取的方法是通过中序遍历的非递归操作。首先看一下二叉树的中序遍历的代码。

```python
    def Inorder(self, root):
        stack = []
        p = root
        while p is not None or len(s) != 0:
            while p is not None:
                stack.append(p)
                p = p.left
            if len(s) != 0:
                p = stack[-1]
                print(p.val)
                p = stack.pop()
                p = p.right
```

然后是整个题的代码。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        #首先判断当前节点是否和p q重合
        if root == p or root == q:
            return root
        #如果两个节点位于不同子树中，直接返回root
        if self.judge_left_or_right(root, p) != self.judge_left_or_right(root, q):
            return root
        else:
            #如果两个节点都位于左子树，对左子树进行递归
            if self.judge_left_or_right(root, p) == 0:
                return self.lowestCommonAncestor(root.left, p, q)
            #如果两个节点都位于右子树，对右子树进行递归
            if self.judge_left_or_right(root, p) == 1:
                return self.lowestCommonAncestor(root.right, p, q)

    #这个函数用来判断一个节点在一个树的左子树还是右子树,方法是使用二叉树的中序遍历的非递归版本，记录下root的值，并且通过flag来标记当前是在左子树还是右子树上，当找到响应的节点的时候返回flag
    def judge_left_or_right(self, root, node):
        stack = []
        p = root
        flag = 0
        while p is not None or len(stack) != 0:
            while p is not None:
                stack.append(p)
                p = p.left
            if len(stack) != 0:
                p = stack[-1]
                #如果当前已经遍历到根节点，就需要将flag变成1，表示当前已经遍历到右子树了。
                if p == root:
                    flag = 1
                if p == node:
                    return flag
                p = stack.pop()
                p = p.right
```

提交之后发现超时了。

## 思路2 十分简洁的递归

```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root or root == p or root == q: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left and not right: return # 1.
        if not left: return right # 3.
        if not right: return left # 4.
        return root # 2. if left and right:

作者：jyd
链接：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/236-er-cha-shu-de-zui-jin-gong-gong-zu-xian-hou-xu/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

