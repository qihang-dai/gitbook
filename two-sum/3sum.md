# 3Sum



Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

## Solution

we can find the triplets easily by include another loop outside 2sum solution, which is O(n2). However the tricky part is know how to avoid duplicate.&#x20;

can sort the list solve the problem? no, there might be duplicate elements.

can we sort every triplet to make it unique? yes, but the T = O(n2 \* n logn) = O(n3logn)

or we can sort the outerlist and thus the pick every time should be the same. use a set to remove duplicate.

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums = sorted(nums)
        hashset = set()
        for i, n in enumerate(nums):
            target = -n
            tmpset = set()
            for j, m in enumerate(nums):
                if j <= i:
                    continue
                match = target - m
                if match in tmpset and (n, match, m) not in hashset:
                    res.append([n, match, m])
                    hashset.add((n, match, m))
                tmpset.add(m)
        return res
```

This pass, but TLE. why?&#x20;

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

In fact, once the nums is sorted, we may not use set to remove duplicate but just use the pointer to push. once we find same element, we push it pointer. We can also utilize the **sorted** information to know we can move left pointer right (get bigger) or move right pointer left(get smaller) to make the sum get close to zero.

### Leetcode solution

```python
def threeSum(self, nums):
    res = []
    nums.sort()
    for i in xrange(len(nums)-2):
        if i > 0 and nums[i] == nums[i-1]:
            continue
        l, r = i+1, len(nums)-1
        while l < r:
            s = nums[i] + nums[l] + nums[r]
            if s < 0:
                l +=1 
            elif s > 0:
                r -= 1
            else:
                res.append((nums[i], nums[l], nums[r]))
                while l < r and nums[l] == nums[l+1]:
                    l += 1
                while l < r and nums[r] == nums[r-1]:
                    r -= 1
                l += 1; r -= 1
    return res
```

### My improved Solution

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums = sorted(nums)

        for i in range(0, len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            left, right = i + 1, len(nums) - 1
            while left < right:
                sum = nums[i] + nums[left] + nums[right]
                #duplicate case special
                if sum == 0:
                    #if we find a match, we begin to look for all the duplicate
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    res.append((nums[i], nums[left], nums[right]))
                    #if all duplicate is scanned, the pointer is at the last duplicate, we move it again
                    left += 1
                    right -= 1
                elif sum > 0:
                    right -= 1
                else:
                    left += 1



        return res
            

```
