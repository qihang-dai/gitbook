# Diameter of Binary Tree

{% embed url="https://leetcode.com/problems/diameter-of-binary-tree/description/" %}

Similar idea as balanced binary tree.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        def maxDepth(root):
            if root == None:
                return 0
            left = maxDepth(root.left)
            right = maxDepth(root.right)
            res = max(left, right) + 1
            return res
        if root == None:
            return 0
        leftdepth = maxDepth(root.left)
        rightdepth = maxDepth(root.right)
        diam = leftdepth + rightdepth
        leftkids = self.diameterOfBinaryTree(root.left)
        righkid = self.diameterOfBinaryTree(root.right)
        
        return max(diam, leftkids, righkid)
  
```

But its super slow. Can we do it like the Balanced Binary Tree? That time we mark every unbalanced as -1, and thus we encode the information into the tree as we traverse once.

```python
def isBalanced(self, root: Optional[TreeNode]) -> bool:
    def dfs(root):
        if not root:
            return 0
        left = dfs(root.left)
        right = dfs(root.right)
        if left == -1 or right == -1:
            return -1
        if abs(left - right) > 1:on
            return -1
        return max(left, right) + 1
    
    return dfs(root) != -1
```

Yes. we can still do it in one run. Remeber how we calculate the biggest diameter: add up the depth of left subtree and depth of right subtree. Then its still an depth problem with some variation. All we need is a max variable when the dfs get the depth of every node from bottom up.

&#x20;In python we need self.var for the static variable.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.ans = 0
        def depth(root):
            if root == None:
                return 0
            left = depth(root.left)
            right = depth(root.right)
            self.ans = max(self.ans, left + right)
            return max(left, right) + 1
        depth(root)
        return self.ans
```
