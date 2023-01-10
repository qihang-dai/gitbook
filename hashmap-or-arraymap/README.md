# HashMap or ArrayMap?

### Ransom Note

{% embed url="https://leetcode.com/problems/ransom-note/description/" %}

The quesiton is simple: count the number of character in each string and see if one is the subset of another:

My thought first: use two hashmap. in python we can use counter for less code

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        mag = Counter(magazine)
        note = Counter(ransomNote)
        for key in note:
            if key not in mag:
                return False
            mag[key] = mag[key] - note[key]
            if mag[key] < 0:
                return False
        return True
```

But then i found its a bit slow in the summary. Can we do better?

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Yes. We can use an 26 length array to store the map of alphabetic characters.&#x20;

1. Array is faster than hashmap for read and index search
2. Sometimes if you are iterate through the 26 array rather than the string length, it will become O(constant) rather than O(n). Though its not the case in this question. I did come to this confusion during my amazon interview and the interviewer pointed this out to me.

```java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] arr = new int[26];
        for (int i = 0; i < magazine.length(); i++) {
            arr[magazine.charAt(i) - 'a']++;
        }
        for (int i = 0; i < ransomNote.length(); i++) {
            if(--arr[ransomNote.charAt(i)-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```
