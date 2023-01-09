# Duplicate, hashset, sliding window, hashmap

Given a string `s`, find the length of the **longest** **substring** without repeating characters.

**Example 1:**

<pre><code><strong>Input: s = "abcabcbb"
</strong><strong>Output: 3
</strong><strong>Explanation: The answer is "abc", with the length of 3.
</strong></code></pre>

{% embed url="https://leetcode.com/problems/longest-substring-without-repeating-characters/description/" %}

I took a look at the hint, saying sliding window, then it should be a simple intuitive solution:

two pointer, begin and end, start from 0

move end

if end find duplicate, move the start till the duplicate is gone.

calculate the max on the go.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        b, e = 0, 0
        hashset = set()
        res = 0
        while e < len(s):
            while s[e] in hashset:
                hashset.remove(s[b])
                b += 1
            else:
                hashset.add(s[e])
            e += 1
            res = max(res, len(hashset))
        return res

## they are the same
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        b, e = 0, 0
        hashset = set()
        res = 0
        while e < len(s):
            if s[e] in hashset:
                hashset.remove(s[b])
                b += 1
            else:
                hashset.add(s[e])
                e += 1
                res = max(res, len(hashset))
        return res
```

It may looks like O(N2). However we are merely move the two pointer along the array for once, so O(N). It is equal to the below java solution

```java
public int lengthOfLongestSubstring(String s) {
    int i = 0, j = 0, max = 0;
    Set<Character> set = new HashSet<>();
    
    while (j < s.length()) {
        if (!set.contains(s.charAt(j))) {
            set.add(s.charAt(j++));
            max = Math.max(max, set.size());
        } else {
            set.remove(s.charAt(i++));
        }
    }
    
    return max;
}
```

Or we can store the index in a hashmap, but its a bit complicated and hurts my brain

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        hashmap = {}
        res = 0
        left = 0
        for index, ch in enumerate(s):
            if ch not in hashmap:
                hashmap[ch] = index
            else:
                if left > hashmap[ch]:
                    left = left
                else:
                    left = hashmap[ch] + 1
                hashmap[ch] = index
            res = max(res, index - left + 1)
            
        return res


```
