# Min stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the `MinStack` class:

* `MinStack()` initializes the stack object.
* `void push(int val)` pushes the element `val` onto the stack.
* `void pop()` removes the element on the top of the stack.
* `int top()` gets the top element of the stack.
* `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with `O(1)` time complexity for each function.

## Solution

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.minstack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if self.minstack:
            curmin = min(self.minstack[-1],val)
        else:
            curmin = val
        self.minstack.append(curmin)
        
    def pop(self) -> None:
        self.stack.pop()
        self.minstack.pop()

    def top(self) -> int:
        return self.stack[-1]
        
    def getMin(self) -> int:
        return self.minstack[-1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

I know i will need a minstack to store the curmin. However i forget how the curmin can be updated properly: once there are pops, the last curmin may not be validated since that curmin is poped out. How can we handle this? dont let curmin stay at the poped out one? i came to find out i can compare the insert val and the last element in minstack. Since minstack and stack pop at the same time, the curmin could be updated with the pop information.

### leetcode solution

We can make this two stack into one two d arry. interesting python where list can be used as stack, thus we can have a 2d stack.

Or we can insert a tuple into stack. The idea is same as my solution, but they use strange quirk to make the code shorter.

```python
class MinStack(object):

    def __init__(self):
        self.stack = []
        
    def push(self, x):
        self.stack.append((x, min(self.getMin(), x))) 
           
    def pop(self):
        self.stack.pop()

    def top(self):
        if self.stack:
            return self.stack[-1][0]
        
    def getMin(self):
        if self.stack:
            return self.stack[-1][1]
        return sys.maxint            
```
