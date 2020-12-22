# Problem234 回文链表

## 题目描述

请判断一个链表是否为回文链表。

## 思路1 将链表的内容表示为数字 正反表示

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        if not head:
            return True
        #num1记录从高位到低位依次表示的数字 以[1,2,3]为例，num1表示的是123
        num1 = 0
        #num2记录从低位到高位依次表示的数字，上面的例子中num2就是321
        num2 = 0
        #通过base表示当前的是1 还是10  还是100
        base = 1
        while head:
            num1 = (num1 * 10 + head.val)
            num2 = (num2 + head.val * base)
            base *= 10
            head = head.next
        return num1 == num2
```

## 思路2 官方的快慢指针

简单的说就是通过快慢指针找到中间节点，然后将后半部分翻转，然后比较，然后再将后半部分反转回来。具体可以看官方的题解。

## 思路3 递归

有一个题解还有一个递归解法，我觉得思路很厉害。

![image-20201222135456342](C:\Users\初泽良\AppData\Roaming\Typora\typora-user-images\image-20201222135456342.png)

比较妙的是那个链表逆序打印。