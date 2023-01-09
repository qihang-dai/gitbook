# LinkedList

## [Palidrome LinkedList](https://leetcode.com/problems/palindrome-linked-list/)

Its 2.5 questions combined:&#x20;

1. find middle of the linkedlist
2. reverse the half of the list. It doesnt matter its the first half or after half.&#x20;
3. split the half list, cause linkedlist, the end point to the null.

<pre><code><strong>#Reverse Code
</strong><strong>class Node:
</strong> 
    # Constructor to initialize the node object
    def __init__(self, data):
        self.data = data
        self.next = None
 
 
class LinkedList:
 
    # Function to initialize head
    def __init__(self):
        self.head = None
 
    # Function to reverse the linked list
    def reverse(self):
        prev = None
        current = self.head
        while(current is not None):
            next = current.next
            current.next = prev
            prev = current
            current = next
        self.head = prev
 
</code></pre>

here the tricky part is still how to reverse. I thought about to reverse the head and head.next and store head.next.next which blows my mind. but it turns out once the head is reverse it need to point to a Null or None node, with a None as prev and curhead as cur, it would be easy to iterate. all we need to do is store head.next as tmp or next and do a loop.



## [Reorder List](https://leetcode.com/problems/reorder-list/description/)

Same idea as the first question. Find middle, split and reverse the after half. One more deeper tricks here is to prevent circle. we store the pos of both next, chain p1 to p2. once p2 is chained to p1, we dont need another pointer to store cur pos but just edit p2 is fine cause we need to do inplace changing. so we chain p2 = tmp1(p1.original next). then we move p1, p2 to the next pos.

To prevent circle, p1 cant be the same as p2.next or p2 != p1.next.  So if the two list length is not even
