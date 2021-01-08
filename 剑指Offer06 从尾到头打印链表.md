# 剑指Offer06 从尾到头打印链表

## 题目描述

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

## 思路1 遍历链表将元素值存入数组，然后将列表翻转

## 思路2 递归法

这其实就是这个题最有意思的地方，怎么用递归法来完成这个题。

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        return self.reversePrint(head.next) + [head.val] if head else []
```

## 思路3 栈

这个题最应该想到的数据结构就应该是栈，栈的先进后出的特性完美适配这个题。

