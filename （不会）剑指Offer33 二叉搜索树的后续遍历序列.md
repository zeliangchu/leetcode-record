# （不会）剑指Offer33 二叉搜索树的后续遍历序列

## 题目描述

输入一个 整数数组 ，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 `true`，否则返回 `false`。假设输入的数组的任意两个数字都互不相同。

## 思路1 递归分治

根据二叉搜索树的定义，可以通过递归，判断所有子树的正确性（即其后序遍历是否满足二叉搜索树的定义),如果所有子树正确，那么此序列为二叉搜索树的后序遍历。

### 递归过程

![image-20210131200327530](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20210131200327530.png)

```python
class Solution:
    def verifyPostorder(self, postorder: List[int]) -> bool:
        def helper(i,j):
            #如果i>=j，说明此节点数量<=1,无需判断正确性，直接返回true
            if i >= j:
                return True
            p = i
            while postorder[p] < postorder[j]:
                p += 1
            m = p
            while postorder[p] > postorder[j]:
                p += 1
            return p == j and helper(i, m-1) and helper(m, j - 1)
        return helper(0, len(postorder) - 1)
```

**复杂度分析：**

- 时间复杂度 O(N^2)  每次调用 helper(i,j) 减去一个根节点，因此递归占用 O(N) ；最差情况下（即当树退化为链表），每轮递归都需遍历树所有节点，占用 O(N) 。
- 空间复杂度 O(N) ： 最差情况下（即当树退化为链表），递归深度将达到 N 。

## 思路2 单调栈