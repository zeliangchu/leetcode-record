# 2020.12.13 力扣第225题 用队列实现栈

## 题目描述

使用队列的操作（只能使用队列的基本操作）来实现栈的入栈、移除栈顶元素、获取栈顶元素、返回栈是否为空的操作。

## 思路一 使用双队列

用两个队列que1和que2来实现队列的功能，que2就是一个备份的作用，当需要弹出栈顶元素的时候，这个时候也就是要将队列que1的最后面的元素弹出，因此我们可以先把最后面的元素的前面所有元素都被备份到que2中，然后弹出最后面的元素，再把其他元素导入到que1中，然后不要忘了清空que2。

具体代码如下：

```python
from collections import deque
class MyStack:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        #使用两个队列来实现
        self.que1 = deque()
        self.que2 = deque()

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.que1.append(x)

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        size = len(self.que1)
        size -= 1#这里先减一是为了保证最后面的元素
        while size > 0:
            size -= 1
            self.que2.append(self.que1.popleft())
            
        
        result = self.que1.popleft()
        self.que1, self.que2= self.que2, self.que1#将que2和que1交换 que1经过之前的操作应该是空了
        #一定注意不能直接使用que1 = que2 这样que2的改变会影响que1 可以用浅拷贝
        return result

    def top(self) -> int:
        """
        Get the top element.
        """
        return self.que1[-1]

    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        #print(self.que1)
        if len(self.que1) == 0:
            return True
        else:
            return False


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

**复杂度分析：**

- 时间复杂度：O(n) n表示队列中元素个数
- 空间复杂度：O(n)

## 思路二 使用单队列

```python
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.queue = collections.deque()


    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        n = len(self.queue)
        self.queue.append(x)
        for _ in range(n):
            self.queue.append(self.queue.popleft())


    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.queue.popleft()


    def top(self) -> int:
        """
        Get the top element.
        """
        return self.queue[0]


    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return not self.queue
```

