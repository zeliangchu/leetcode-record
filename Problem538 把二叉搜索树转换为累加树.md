# Problem538 把二叉搜索树转换为累加树

## 题目描述

![image-20210105152732417](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210105152732417.png)

## 思路

使用定义一种新的遍历顺序 右子节点-根-左子节点  

转换过程中的规律就是这个顺序然后把当前访问节点的值加上上一个访问节点的值。

直接上代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    num = 0
    def convertBST(self, root: TreeNode) -> TreeNode:
        self.MyOrder(root)
        return root
    
    def MyOrder(self, root):
        if not root:
            return 
        self.MyOrder(root.right)
        root.val += self.num
        self.num = root.val
        self.MyOrder(root.left)
```

**复杂度分析：**

- 时间复杂度：O(n) 其中n是二叉搜索树中的节点数目，每个节点恰好遍历一次
- 空间复杂度:O(n) 最坏情况下树呈链状