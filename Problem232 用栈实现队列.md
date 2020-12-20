# Problem232 用栈实现队列

## 题目描述

请你仅使用两个栈实现先进先出队列。

## 思路

```python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack = []
        self.help_stack = []

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.stack.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        while len(self.stack) > 1:
            self.help_stack.append(self.stack.pop())
        val = self.stack.pop()
        while self.help_stack:
            self.stack.append(self.help_stack.pop())
        return val

    def peek(self) -> int:
        """
        Get the front element.
        """
        while self.stack:
            self.help_stack.append(self.stack.pop())
        val = self.help_stack[-1]
        while self.help_stack:
            self.stack.append(self.help_stack.pop())
        return val

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return len(self.stack) == 0


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

我这个解法不满足均摊复杂度的要求，具体可以看题解。