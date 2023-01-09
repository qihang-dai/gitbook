# Stack

## [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for b in s:
            if b == '(':
                stack.append(")")
            elif b == '[':
                stack.append("]")
            elif b == "{":
                stack.append("}")
            else:
                if stack and stack[-1] == b:
                    stack.pop()
                else:
                    return False
        print(stack)
        return len(stack) == 0
```

I made a mistake first: not using elif but it lead the else part gets excuted as long as its not the third case. The idea is simple:

scan the left bracket.

run into right bracket, see if its a match. if match the pop, if not match or nothing to match(stack empty) return False.  so we try to validate every right bracket. This can be write into a shorter form via:

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for b in s:
            if b == '(':
                stack.append(")")
            elif b == '[':
                stack.append("]")
            elif b == "{":
                stack.append("}")
            else:
                if not stack or stack.pop() != b:
                    return False
        print(stack)
        return not stack
```

It really doesnt matter its shorter or not. Its just utilize the `or`  for the empty case check

At last if all right bracket is matched, watch out if there is extra left bracket.

