# 剑指Offer18 删除链表的节点

## 题目描述

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

## 思路

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def deleteNode(self, head: ListNode, val: int) -> ListNode:
        dummy_head = ListNode(None)
        dummy_head.next = head
        p = head
        pre = dummy_head
        while p.val != val:
            pre = p
            p = p.next
        pre.next = p.next
        return dummy_head.next
```

