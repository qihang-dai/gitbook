# Binary Tree Level Order Traversal

We have talked enough about DFS. Now It's BFS time.&#x20;

### BFS thinking process:

1. push the first level root
2. push every next level child
3. again and again

Several thing need to be notice:

1. how to do the leve by level? We can loop the current level nodes inside the queue, so `while queue: for node in queue`.   However the queue size is always changing, so we need to store the size of one level node along, we cant do `for i in range(len(queue))` cause it will get queue.size every i in iteration. thus we should do a `size = len(queue)` at first
2. Since its tree, when you try to push the kid, you might push the Null node. So need to do if check

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        q = deque([root])
        res = []

        while q:
            tmp = []
            size = len(q) # this is critical
            # print(size)
            for i in range(size):
                node = q.popleft()
                tmp.append(node.val)
                if node.left: q.append(node.left)
                if node.right: q.append(node.right)
            res.append(tmp)
            
        return res
        
```

## Review

I always forget how to properly write the BFS code, but i do not know why.  So lets explain how to implement BFS.

we need a queue. so deque

We end till all node out, so while q

we push the upper node, and get out of all the upper node while push in the new node. we need it level by level inside while q. so we need anoter loop. how to know the q is in different level? We can have the node count of the level we try to get out. Since the queue size is changing as we push into, we need to store size = len(q)

I will try to improve this later

