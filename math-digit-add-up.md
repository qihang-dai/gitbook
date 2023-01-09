# Math, Digit add up

## [Add Binary](https://leetcode.com/problems/add-binary)

Return the result of adding up two binary digit string, like "11" + "1" = "100"

### My long simulation solution:

The idea is to watch out for the carry, and do brutal simulation

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        lena = len(a)
        lenb = len(b)
        i, j = lena - 1, lenb - 1

        st = []
        carry = 0
        while i >= 0 and j >= 0:
            cursum = int(a[i]) + int(b[j]) + carry
            if cursum == 2:
                carry = 1
                st.append(0)
            elif cursum == 3:
                carry = 1
                st.append(1)
            else:
                carry = 0
                st.append(cursum)
            i -= 1
            j -= 1
        print(st)

        while i >= 0:
            cursum = int(a[i]) + carry
            if cursum == 2:
                carry = 1
                st.append(0)
            else:
                carry = 0
                st.append(cursum)
            i -= 1

        while j >= 0:
            cursum = int(b[j]) + carry
            if cursum == 2:
                carry = 1
                st.append(0)
            else:
                carry = 0
                st.append(cursum)
            j -= 1

        if carry == 1:
            st.append(carry)
        return "".join(map(str,st[::-1]))

```

### Leetcode Solution

same idea, shorter version, Only one loop. Make things short is also a useful mindset, cause sometimes you cant make things run with long logic, you have to solve them in one loop

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        res = ""
        i, j, carry = len(a) - 1, len(b) - 1, 0
        while i >= 0 or j >= 0:
            sum = carry;
            if i >= 0 : sum += ord(a[i]) - ord('0') # ord is use to get value of ASCII character
            if j >= 0 : sum += ord(b[j]) - ord('0')
            i, j = i - 1, j - 1
            carry = 1 if sum > 1 else 0;
            res += str(sum % 2)

        if carry != 0 : res += str(carry);
        return res[::-1]
```

1. First thing stuggle with me is one string maybe shorter than another. Thus we have to use extra loop to handle the longer string. Here we can see its possible to use if inside a loop to do both condition in one loop.
2. carry is 1 if cursum > 1. we have another if
3. digit to append is 0 if cursum is odd else 1. We have another if, or mod operator. **Manytimes the `if` statement can be translated into some simple operation.**

**and dont forget to add the last carry if its not zero**

```python
while a > 0 or b > 0:
    if a > 0:
        ...
    if b > 0:
        ...
    carry = 1 if cursum > 1 else 0
    res += str(cursum % 2)
```
