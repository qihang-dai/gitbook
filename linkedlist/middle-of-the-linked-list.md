# 😂 Middle of the Linked List

[Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list)

_Given the `head` of a singly linked list, return the middle node of the linked list._

_If there are two middle nodes, return **the second middle** node_.

Slow and fast pointer, nothing else;

{% code lineNumbers="true" %}
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow, fast = head, head
        while fast != None and fast.next != None:
            slow = slow.next
            fast = fast.next.next

        return slow
```
{% endcode %}

But let us revisit some thing:

Why line 9? fast would stop at None first.  Since it is doing `fast.next.next`, we dont want fast.next == None, and we want it stop when its at the last node.`fast != None cause we want it stop one .next.next jump into the None after the endings.`

How does it return the middle and return the second middle when there are two middle nodes

1. For  1 > 2 > 3 > 4 > 5,&#x20;
2. fast move to 3, slow move to 1
3. fast to 5, slow to 3, fast.next ==None, the loop stop

Because for odd length node list, start from the head,  the rest nodes amount is even. fast move twice would reach the last node eventually

But for Even length node list, the rest amount is odd, fast move twice and would reach the None after the last node. Consider that None as one extra node to the orignal list, the new list length is odd and the rest is even, thus returns the second middle nodes.

## [Reorder List](https://leetcode.com/problems/reorder-list/)

Another more complex question on head of this&#x20;

You are given the head of a singly linked-list. The list can be represented as:

```
L0 → L1 → … → Ln - 1 → Ln
```

_Reorder the list to be on the following form:_

```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

&#x20;

**Example 1:**

<figure><img src="https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg" alt=""><figcaption></figcaption></figure>

<pre><code>Input: head = [1,2,3,4]
<strong>Output:
</strong> [1,4,2,3]
</code></pre>



### Solution:

{% code lineNumbers="true" %}
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        slow, fast = head, head
        while fast != None and fast.next != None:
            slow = slow.next
            fast = fast.next.next
        
        cur = slow.next #have to split the list without same 
        # 1 -> 2 -> 3 -> 4
        #first half: 1 2 3
        # second half: 4
        # if cur = slow (midpoint)
        
        prev = None
        while cur != None:
            next = cur.next
            cur.next = prev
            prev = cur
            cur = next
        
        slow.next = None
        # 1 -> 2 -> 3 -> None
        # 1 -> 2 -> 3 <- 4


        p1 = head
        p2 = prev
        print(p1)
        print(p2)
        while p1 != None and p2 != None:

            tmp1 = p1.next
            tmp2 = p2.next
            
            #p1 chain to p2, p2 chain to p1.next.
            p1.next = p2
            p2.next = tmp1
            
            #move the saved pos
            p1 = tmp1
            p2 = tmp2

```
{% endcode %}

The idea:

1. Find the middle
2. Split, reverse the after half
3. Merge two list one by one

We already have the find middle code. Here is the reverse code:

```python
    prev = None
    while cur != None:
        next = cur.next
        cur.next = prev
        prev = cur
        cur = next
```

The MIT part for reverse is to set a virtual prev = None as a placeholder so its better to iterate through. Otherwise the head situation would be different from other nodes, which would be tricky to handle. **This virtual prev node is an example of make special case generalizble in one same loop.**

**Another tricky part is to know what the split may looks like:**

If the list is odd amount, one middle node like 1,2,3,4,5:

1. Middle is 3, which slow is at
2. first half 1,2,3; second half 3,4,5
3. reverse second half to 5,4,3
4. Then merge, there would be one same 3 which brings cycle

If the list is even, two middle node like 1,2,3,4,5,6

1. second mid is 4, where slow is at
2. first half 1, 2, 3,4, second half  4, 5, 6

To prevent circle, p1 cant be the same as p2.next or p2 != p1.next.  So if we done line 16 cur = slow.next and leave two list have no common node, we should avoid this create cycle bug

****

