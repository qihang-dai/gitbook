# Longest Palindrome

Its very similar to what i failed in the Doordash OA. The difference is that question is trying to find the longest palindrome from the longest palindrome of two string, which brings extra complexity from the merge process.  This one is a simple version of that problem.

Here what we need to do is just count even and odd. We can choose any even part out from the odd count. But at last only one character could be odd, so we add one plus.

### Mysolution:

```python
class Solution:
    def longestPalindrome(self, s: str) -> int:
        count = Counter(s)
        print(count)

        maxOdd = 0
        alleven = 0
        extra = 0
        for key in count:
            if count[key] % 2 != 0:
                alleven = alleven + count[key] - 1
                extra = 1
            else:
                alleven = alleven + count[key]
        
        res = alleven + extra
        return res
```

### Leetcode Solution:

```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: int
        """
        hash = set()
        for c in s:
            if c not in hash:
                hash.add(c)
            else:
                hash.remove(c)
        # len(hash) is the number of the odd letters
        return len(s) - len(hash) + 1 if len(hash) > 0 else len(s)
```

They use hashset add and remove to count the even and odd, which is the same idea.

### The doordash variant:

Here we would need to return the lexicographical order smallest string(also known as lexical order, or dictionary order).&#x20;

What we need to do is still count, but take the two string's count together into consideration. Also to make lexical smallest string, we can simply loop from 'a' to 'z'  to append string, and make an arraymap from 'a' to 'z' rather than use hashmap. **Arraymap should always be better when doing with fixed amount of keys.**  &#x20;

From a to z, if count in both string is even, simply append it. if one count is odd, count // 2 (floor) would solve the problem. So we loop all the count and add up to count1\[c] // 2 + count2\[c]// 2

we would make the smallest odd to be the center. So once there is an odd, if both str count is odd, we make that center for both str palindrome. But if only one str count is odd first, we made that center. It really doesnt matter which one odd character to be the cetner as long as it meets the requirment.

then `halfpalindrome + centerChar + halfReverse` we have the solution
