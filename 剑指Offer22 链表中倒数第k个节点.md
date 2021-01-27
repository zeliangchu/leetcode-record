# 剑指Offer22 链表中倒数第k个节点

## 题目描述

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

## 思路 两次遍历

```python
class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        count = 0
        p = head
        while p:
            count += 1
            p = p.next
        left = count - k
        p = head
        while left:
            left -= 1
            p = p.next
        return p
```

## 思路2 快慢指针

```python
class Solution:
    def getKthFromEnd(self, head: ListNode, k: int) -> ListNode:
        fast, slow = head, head
        while fast and k:
            fast = fast.next
            k -= 1
        while fast:
            fast = fast.next
            slow = slow.next
        return slow
```

这个题确实是快慢指针一个很经典的题，链表必须考虑到的一个方法就应该是快慢指针。