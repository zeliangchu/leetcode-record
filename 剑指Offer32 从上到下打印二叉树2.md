# 剑指Offer32 从上到下打印二叉树2

## 题目描述

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。

## 思路

层序遍历，只不过需要输出一层。

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        res, queue = [], collections.deque()
        queue.append(root)
        while queue:
            cur_layer = []
            for i in range(len(queue)):
                node = queue.popleft()
                cur_layer.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            res.append(cur_layer)
        return res
```

这里的一个关键是for i in range(len(queue)) 这个循环。