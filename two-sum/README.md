# Two Sum

Description: find two value in the list that add up to the target, return the two index

We know its hashmap: you search the index by the value.&#x20;

The trouble is: when there is duplicate, or one value can double into the target

My solution

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        res = []
        mapping = {}
        for i in range(len(nums)):
            mapping[nums[i]] = i
        
        print(mapping)
        for i in range(len(nums)):
            cur = nums[i]
            match = target - cur
            matchIndex = mapping[match] if match in mapping else -1
            if matchIndex != i and matchIndex != -1:
                return [i, matchIndex]
                
```

This is two pass two iteration O（2n). You construct the map while one loop and see if the index is the same.

**But you can acc do it in one pass, construct the map as you go**. So:

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement in hashmap:
                return [i, hashmap[complement]]
            hashmap[nums[i]] = i
```

if make the map on the fly, you can avoid the index duplicate like you construct one time, and avoid to look at the same value twice. dict would make two in one, but if you do it on the fly, you are still looking one by one.
