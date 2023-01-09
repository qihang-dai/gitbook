# Contains Duplicate

This question is simple, but this leetcode discussion provide great idea about how to consider trade offf between space and time complexity on this simple question.

{% embed url="https://leetcode.com/problems/contains-duplicate/solutions/60858/possible-solutions/" %}

My solution:

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        hashset = set()
        for n in nums:
            if n in hashset:
                return True
            hashset.add(n)
        return False
```

But it can be one line in python:

```python
return len(nums) != len(set(nums))
```
