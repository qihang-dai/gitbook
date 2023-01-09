# Balanced Binary Tree

### [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

The Base case of many tree question: Find Depth (depth is related to the longest subtree)

Code is simple: recursively get the left and right, the depth is max(l, r) + 1. One beat the end case None, return 0.

```python
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root:
            left = self.maxDepth(root.left)
            right =self.maxDepth(root.right)
            return max(left, right) + 1
        return 0
    
    #same as the below
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root == None: 
            return 0
        left = self.maxDepth(root.left)
        right =self.maxDepth(root.right)
        return max(left, right) + 1
    
    ## they can all be see as if else's shorter version because of return
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if root:
            left = self.maxDepth(root.left)
            right =self.maxDepth(root.right)
            return max(left, right) + 1
        elif root == None:
            return 0
```



### Balanced Binary Tree

{% embed url="https://leetcode.com/problems/balanced-binary-tree/description/" %}

### Top down brutal force

though the dfs find depth function is bottom up

{% code lineNumbers="true" %}
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def dfs(root):
            if not root:
                return 0
            left = dfs(root.left)
            right = dfs(root.right)
            return max(left, right) + 1
        
        if not root: # dont need to check the null root is return ture
            return True 
        left = dfs(root.left)
        right = dfs(root.right)
        if abs(left - right) > 1:
            return False
        if not self.isBalanced(root.left) or not self.isBalanced(root.right):
            return False
        return True

```
{% endcode %}

The question is to find whether there is a root has subtrees unbalanced (more than 1 depth differ sub tree).

Straightforward brutal force: find depth of every node. for each node check the left depth and right depth,  and check everynode.

empty node dont need to be checked. so return true unless other node is false. same result with the following code

```python
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if root:
            left = dfs(root.left)
            right = dfs(root.right)
            if abs(left - right) > 1:
                return False
            if not self.isBalanced(root.left) or not self.isBalanced(root.right):
                return False
        return True
```

However there is duplicate computation when check everynode, the depth is calculated again. what if we store the depth inside a hashmap? maybe boost a little bit like dynamic programming.



### bottom up:

or we can acc know the balance while calculate the depth. If the depth is unbalance we record it into -1. why bother calculate depth and traverse the tree so many times? and why bother you record the depth in hashmap while you can do it on the go?

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

