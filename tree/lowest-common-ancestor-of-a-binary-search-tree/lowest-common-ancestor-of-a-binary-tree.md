# Lowest Common Ancestor of a Binary Tree

```python
```

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/description/" %}

## Update Self Solved it second time:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root == None or root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if (left == p and right == q) or (left == q and right == p):
            return root
        if left:
            return left
        else:
            return right
        
```

so we use the postorder recursion to touch the node.  if we postorderly find both node in left and right side, then that node is the ancestor.

## Fail to solve first time

> It's recursive and expands the meaning of the function. If the current (sub)tree contains both p and q, then the function result is their LCA. If only one of them is in that subtree, then the result is that one of them. If neither are in that subtree, the result is null/None/nil.

```python
        def dfs(root):
            if root == p:
                return p
            if root == q:
                return q
            left = dfs(root.left)
            right = dfs(root.right)
            if left == p and right == q:
                return root
        res = dfs(root)
        return res
```

My solution at first: the question is: what happens when p is the ancestor of q?

answer: return p would be enough

but, how can we know that? in my code the dfs stop at when it finds p and will never reach q.

its okay acc. since p exits as the question said, if q is not found, just return P.

but i have somer left and right issue. what if the p is just the base root without any left right?

special case is no good. its like put fix for inifinite uncomplete design



you are confusing yourself: the truth is: if find p or q, return. If not, return nothing. if find p first, and q is not reached, okay p is the ancestor.

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if root in (None, p, q):
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left and right:
            return root
        return (left, right)[left == None]
```

the last line can acctually be:&#x20;

```python
return root if left and right else left or right
```

but its not a good way to debug. we wont know which way to go when bugs come out.
