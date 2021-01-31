# （不会）剑指Offer31 栈的压入、弹出序列+Problem946 验证栈序列

## 题目描述

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

## 思路

这个题比较容易想到使用一个栈来模拟栈的压入、弹出操作。关键是模拟的过程，这个模拟的过程中最关键的是pop的过程，push的操作通过遍历pushed数组就可以了，但是在压入的过程中需要判断是否需要弹出，怎么判断这个弹出的操作是关键，因为可能接连弹出好几个，也有可能这个弹出之后接下来又是压入。我们使用一个指针来指向popped中元素的位置，当碰到栈顶的元素和该指针指向的元素相等的时候，就说明需要弹出了，这个时候就栈顶的元素弹出，同时指针向后移动。

```python
class Solution:
    def validateStackSequences(self, pushed: List[int], popped: List[int]) -> bool:
        stack, i = [], 0
        for num in pushed:
            stack.append(num) # num 入栈
            while stack and stack[-1] == popped[i]: # 循环判断与出栈
                stack.pop()
                i += 1
        return not stack
```

