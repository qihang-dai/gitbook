# Tree

## [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

### My Solution:

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def invert(root):
            if root:
                invert(root.left)
                invert(root.right)
                root.left, root.right = root.right, root.left
        invert(root)
        return root
```

### Discussion Solution

```python
def invertTree(self, root):
    if root:
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
```

### Difference

My recursive function have no return value. Thus i have to define a new recursive function other than use the original solution function that has a return value.

But how to do recursive with a return value? Just go deeper, return until its None or Null. So this recursive goes like this:

1. go deeper until the root is None
2. the last leave root change the two None root. go back one level up
3. the father node exchange the leave node.
4. Recursively, all node is exchanged. Its like postorder traversal. The `return` is the `print` if doing postorder print tree.

BFS, DFS should also works

```python
# recursively
def invertTree1(self, root):
    if root:
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
        
# BFS
def invertTree2(self, root):
    queue = collections.deque([(root)])
    while queue:
        node = queue.popleft()
        if node:
            node.left, node.right = node.right, node.left
            queue.append(node.left)
            queue.append(node.right)
    return root
    
# DFS
def invertTree(self, root):
    stack = [root]
    while stack:
        node = stack.pop()
        if node:
            node.left, node.right = node.right, node.left
            stack.extend([node.right, node.left])
    return root
```



