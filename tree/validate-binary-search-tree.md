# Validate Binary Search Tree

Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.



## Solution:



At first i write wrong code: i only think about if the left node is less than root and right node is larger than root. for BST the entire left subtree need to be smaller. **I spent around 20 min and find my code grows over and over complicated. Then i know i am in the wrong direciton**

Then i realize inorder BST tree would return a increasing array. and here is the soluion

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        res = []
        def inorder(root):
            if root:
                inorder(root.left)
                res.append(root.val)
                inorder(root.right)
        
        inorder(root)
        for i in range(1, len(res)):
            if res[i] <= res[i - 1]:
                return False
        return True
```

But i keep thinking how can i dont use the array but do the work along the recursion. I failed to struggle but look at the solution. the idea is simple, i should have eat some breakfast and work it out myself: in inorder you can have a parameter that stores the last value, whether its a global variable or a parameter passed in the function. and you can compare it to the cur node val.

````python
```python3
class Solution:
    pre = -math.inf
    def isValidBST(self, root: Optional[TreeNode], pre = -math.inf) -> bool:
        if not root:
            return True
        left = self.isValidBST(root.left, pre)
        if pre < root.val:
            pre = root.val
        else:
            return False
        right = self.isValidBST(root.right, pre)
        
        return left and right
```
````

However this is not working. Why? the pre across the traversal, but acrossed the level. so you have to use global variable. Due to the ugly unconvenicence of python global variable, you should do another function and declare it nonlocal inside it. or you can write a init function iside the clasee.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        pre = -float("INF")
        def inorder(root):
            nonlocal pre
            if not root:
                return True
            left = inorder(root.left)
            if pre < root.val:
                pre = root.val
            else:
                return False
            right = inorder(root.right)
            return left and right
        
        return inorder(root)
```

Its still ugly, is there a way we can do it by pass parameters inside function? yes

Use recursion. Pass down two parameters: `lessThan` (which means that all nodes in the the current subtree must be smaller than this value) and `largerThan` (all must be larger than it). Compare root of the current subtree with these two values. Then, recursively check the left and right subtree of the current one. Take care of the values passed down.

```python
class Solution(object):
    def isValidBST(self, root, lessThan = float('inf'), largerThan = float('-inf')):
        if not root:
            return True
        if root.val <= largerThan or root.val >= lessThan:
            return False
        return self.isValidBST(root.left, min(lessThan, root.val), largerThan) and \
               self.isValidBST(root.right, lessThan, max(root.val, largerThan))
```

as we can see, there are nothing about inorder. Its acctually preorder. we do the two recursion at the end and do the judege code at first. However it doesnt matter as long as you are traversing. you pass parameter that can compare with the left subtree and right subtree and thats all you need for this problem.

surprisingly inorder, preoder or postorder all works. No matter what order you are traversing, the value is passed from top  to down. so all would work

