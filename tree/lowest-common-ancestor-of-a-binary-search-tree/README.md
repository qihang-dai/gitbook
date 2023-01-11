# Lowest Common Ancestor of a Binary Search Tree

{% embed url="https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/" %}

I am too hungry so didnt see the "BST" tag. since its a search tree, it is much easier.\
\
The find common ancestor in normal binary tree is acctually a little harder. I failed to solve it though.

in bst its an easy problem: both val < rootval then left subtree, both larger then right subtree. p.val and q.val in two side of the root.val, then in different subtree, different side, then this root is the lowest common ancestor.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root

```
