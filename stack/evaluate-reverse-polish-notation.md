# Evaluate Reverse Polish Notation



**150. Evaluate Reverse Polish Notation**

Evaluate the value of an arithmetic expression in [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse\_Polish\_notation).

Valid operators are `+`, `-`, `*`, and `/`. Each operand may be an integer or another expression.

**Note** that division between two integers should truncate toward zero.

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.



**Example 1:**

<pre><code><strong>Input: tokens = ["2","1","+","3","*"]
</strong><strong>Output: 9
</strong><strong>Explanation: ((2 + 1) * 3) = 9
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: tokens = ["4","13","5","/","+"]
</strong><strong>Output: 6
</strong><strong>Explanation: (4 + (13 / 5)) = 6
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
</strong><strong>Output: 22
</strong><strong>Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
</strong>= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
</code></pre>

## Solution

Simple idea: push **on the go. I feel ON THE GO is a frequently used techiniques for leetcode question. Since when you process somthing on the go, you are reduce the code complexity and there will be less case you need to consider. On the go provide a time series like pattern, you will only need to think about whats now and before, and the after would reuse your results on the go.**

push into stack on the go, if number push in, in operators do operation for the 2 stack.pop, then pushback. the rest in the stack is the result

### My code:

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        def operation(op:str, t1, t2):
            t1 = int(t1)
            t2 = int(t2)
            if op == "+":
                return t1 + t2
            elif op == "-":
                return t1 - t2
            elif op == "/":
                return int(t1 / t2)
            elif op =="*":
                return t1 * t2
            else:
                print("unknown operators")

        # print(6 // -132)
        # print(6/ -132)
        stack = []
        operators = set(["+", "-", "*","/"])
        for t in tokens:
            if t not in operators:
                stack.append(t)
            else:
                t2 = stack.pop()
                t1 = stack.pop()
                res = operation(t, t1, t2)
                stack.append(res)
            # print(stack)
        return stack[0]
            
```

Something need to notice:

1. input is string, so we need to convert to int(t)
2. **Note** that division between two integers should truncate toward zero.  exp: 6 / -132 = -0.4...., \
   if use floor division, when there is negative interger, it will floor to the smaller one, so 6 // -132 = -1. here we should do int(6/-132) = int(-0.4), which floor to the larger digit, the 0.
3. and thats it.

### Leecode simpler solution

This one was shorter. But it is optimized and wont show the entire thinking process.

```python
class Solution(object):
    def evalRPN(self, tokens):
        stack = []
        for t in tokens:
            if t not in "+-*/":
                stack.append(int(t))
            else:
                r, l = stack.pop(), stack.pop()
                if t == "+":
                    stack.append(l+r)
                elif t == "-":
                    stack.append(l-r)
                elif t == "*":
                    stack.append(l*r)
                else:
                    stack.append(int(float(l)/r))
        return stack.pop()
```
