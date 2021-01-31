# 剑指Offer32 从上到下打印二叉树3

## 题目描述

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

##  思路 调用reversed方法

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        res, queue = [], collections.deque()
        queue.append(root)
        level = 1
        while queue:
            cur_layer = []
            for i in range(len(queue)):
                node = queue.popleft()
                cur_layer.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            if level & 1:
                res.append(cur_layer)
            else:
                res.append(list(reversed(cur_layer)))
            level += 1
        return res
```

## 思路 通过双端队列的巧妙变换来做

学会巧妙运用双端队列的popleft pop append appendleft

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return []
        res, queue = [], collections.deque()
        queue.append(root)
        level = 1
        while queue:
            cur_layer = []
            for i in range(len(queue)):
                if level & 1:
                    node = queue.popleft()
                    if node.left:
                        queue.append(node.left)
                    if node.right:
                        queue.append(node.right)
                else:
                    node = queue.pop()
                    if node.right:
                        queue.appendleft(node.right)
                    if node.left:
                        queue.appendleft(node.left)
                cur_layer.append(node.val)
            res.append(cur_layer)
            level += 1
        return res
```

