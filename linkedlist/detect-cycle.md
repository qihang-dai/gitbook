# Detect Cycle

{% embed url="https://leetcode.com/problems/linked-list-cycle/" %}

{% code title="MyOriginalSolution" %}
```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        hashset = set()
        while head != None:
            if head in hashset:
                return True
            hashset.add(head)
            head = head.next
        return False
```
{% endcode %}

But acc we should use two pointer and use space 1. Why? because this in memory hashset may enocunter out of memory limit if the linkedlist is pretty long.

One fun solution is rewrite the value to a never occuring value, which is interesting but not practical in real work situation. Nobody would want you to rewrite the database

{% code title="TwoPointer" overflow="wrap" lineNumbers="true" %}
```python
// # Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow, fast = head, head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow is fast:
                return True
        return False
```
{% endcode %}

