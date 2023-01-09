# Reverse LinkedList

{% embed url="https://leetcode.com/problems/reverse-linked-list/description/" %}

Simplest version of linkedlist. The key is to store the next node's reference in another variable, then we can move the two pointer through the list.

Another thing need to know is the last element is acc point to None rather than point to nothing (literally equal\~) So set `prev = None` is a gimmick.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = head
        prev = None
        while cur != None:
            next = cur.next
            cur.next = prev
            prev = cur
            cur = next
        return prev
```
