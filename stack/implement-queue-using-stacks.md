# Implement Queue using Stacks

{% embed url="https://leetcode.com/problems/implement-queue-using-stacks/description/" %}

How can two stack become queue? put in one stack and pour into another when pop or peek.

But we cant pour into another every time and then pour back. we can pour once, and till the second stack pop into empty, we can pop again. We can reuse the poured content until all elements are dequed.

### code

```python
class MyQueue:

    def __init__(self):
        self.ins = []
        self.out = []
        

    def push(self, x: int) -> None:
        self.ins.append(x)

    def pop(self) -> int:
        if not self.out:
            while self.ins:
                self.out.append(self.ins.pop())
        return self.out.pop()
        

    def peek(self) -> int:
        if not self.out:
            while self.ins:
                self.out.append(self.ins.pop())
        return self.out[-1]
        

    def empty(self) -> bool:
        return not (self.ins or self.out)
        


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```
