# Problem297 二叉树的序列化和反序列化

## 题目描述

序列化是将一个数据结构或者对象转换为连续比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境中，采用相反的方法重构得到原数据。

请设计一个算法来实现二叉树的序列化和反序列化。这里不限定序列化或者反序列化的执行逻辑，只要保证一个二叉树可以被序列化一个字符串并且将这个字符串反序列化为原来的树结构即可。

## 思路1 使用层序遍历，并且在序列化的时候记录为完全二叉树

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        res = []
        queue = [root]
        while queue:
            next_layer = list()
            layer = list()
            for node in queue:
                if not node:
                    layer.append(None)
                    next_layer.append(None)
                    next_layer.append(None)
                    continue
                next_layer.append(node.left)
                next_layer.append(node.right)

                layer.append(node.val)
            if layer:
                res.extend(layer)
            if next_layer.count(None) != len(next_layer):
                queue = next_layer
            else:
                queue = []
        ans = ','.join(map(str, res))#注意join方法要求list里面的元素是str类型，而不能是int类型
        #ans = '[' + ans + ']'
        #print(ans)
        return ans

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        n = len(data)
        #去掉首尾的[ ]
        #tmp = data[1:n-1]
        helper = data.split(',')
        return self.creatTree(helper)
    
    def creatTree(self, data):
        #这个写法是基于完全二叉树的
        li = []
        for a in data:  # 创建结点
            if a != "None":
                node = TreeNode(int(a))
            else:
                node = None
            li.append(node)
        parentNum = len(li) // 2 - 1
        for i in range(parentNum+1):
            leftIndex = 2 * i + 1
            rightIndex = 2 * i + 2
            if li[i] is None:
                continue
            li[i].left = li[leftIndex]
            if rightIndex < len(li):  # 判断是否有右结点， 防止数组越界
                li[i].right = li[rightIndex]
        return li[0]

# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```

前面层序遍历的那个地方主要是参考之前写的层序遍历的那个题的代码。

这个思路最后超时了，因为如果有斜着的二叉树的话，这样浪费了很多时间和空间上的代价。

在评论区看到了一个很好懂的代码。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        s = ''
        queue = deque()
        queue.append(root)
        while queue:
            node = queue.popleft()
            if node:
                s += str(node.val)
                queue.append(node.left)
                queue.append(node.right)
            else:
                s += 'n'
            s += ' '
        return s
    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        tree = data.split()
        print(tree)
        if tree[0] == "n":
            return None
        queue = deque()
        root = TreeNode(int(tree[0]))
        queue.append(root)
        i = 1
        while queue:
            node = queue.popleft()
            if node is None:
                continue
            node.left = TreeNode(int(tree[i])) if tree[i] != 'n' else None
            node.right = TreeNode(int(tree[i + 1])) if tree[i + 1] != 'n' else None
            i += 2
            queue.append(node.left)
            queue.append(node.right)
        return root
```

![image-20201226183000363](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201226183000363.png)

对于上面的一棵树，它的序列化结果为：1 2 3 n n 4 5 6 7 n n n n n n  这样一个字符串，然后进行反序列化，也就是把每个叶子节点的两个空子结点加进去了。然后反序列化的时候维护了一个i指针，来进行遍历。

序列化和反序列化的过程都通过BFS来实现，使用队列，挺巧妙的。