# 剑指Offer30 包含min函数的栈

## 题目描述

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

## 思路

这个题的难点在于求最小值的操作正常情况下只能通过遍历数组来解决，要想通过常数时间，需要将通过辅助栈来解决。

- 第一个栈用于存储所有元素，保证push pop top等操作的正常逻辑。
- 第二个栈存储第一个栈中所有**非严格降序**的元素，保证第一个栈中的最小元素始终对应第二个栈的栈顶元素。

<img src="https://pic.leetcode-cn.com/f31f4b7f5e91d46ea610b6685c593e12bf798a9b8336b0560b6b520956dd5272-Picture1.png" alt="Picture1.png" style="zoom:50%;" />

注意非严格递增序列，举例来说，如果有两个3，第二3也必须加到第二个栈中。

```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if not self.min_stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)

    def pop(self) -> None:
        if self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def min(self) -> int:
        return self.min_stack[-1]
```

