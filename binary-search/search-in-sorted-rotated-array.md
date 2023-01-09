# Search in Sorted Rotated Array

33\. Search in Rotated Sorted ArrayMedium19.3K1.2KCompanies

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [4,5,6,7,0,1,2], target = 0
</strong><strong>Output: 4
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [4,5,6,7,0,1,2], target = 3
</strong><strong>Output: -1
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: nums = [1], target = 0
</strong><strong>Output: -1
</strong></code></pre>

## solution

I had this problem in one of my company online assesment in 2021 October. I remember its some kinda of codesginal test for doordash or tiktok. Sadly I spent around 40 minutes on it trying to write a binary search problem. I was not very clear about how binary search works that time, so the debugging kills me.

Clearly in my mind, first we need to find the rotate point (pivot), then two ways:

1. translate the middle point with the pivot. `(mid + pivot) % len(array)`
2. do binary search both side. easy life

### 2. search in seperate side

````python
```python3
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        pivot = 0
        for i in range(1, len(nums)):
            if nums[i] <= nums[i - 1]:
                pivot = i
                break
        left = self.bfs(0, pivot - 1, nums, target)
        right = self.bfs(pivot, len(nums) - 1, nums, target)
        print(left, right)
        if left != -1:
            return left
        if right != -1:
            return right
        return -1
    
    def bfs(self, left, right, nums, target):
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        return -1

```
````

### 1. tranlsate the array ( recover the old array)

> For those who struggled to figure out why it is realmid=(mid+rot)%n: you can think of it this way: If we want to find realmid for array \[4,5,6,7,0,1,2], you can shift the part before the rotating point to the end of the array (after 2) so that the sorted array is "recovered" from the rotating point but only longer, like this: \[4, 5, 6, 7, 0, 1, 2, 4, 5, 6, 7]. The real mid in this longer array is the middle point between the rotating point and the last element: (rot + (hi+rot)) / 2. (hi + rot) is the index of the last element. And of course, this result is larger than the real middle. So you just need to wrap around and get the remainder: ((rot + (hi + rot)) / 2) % n. And this expression is effectively (rot + hi/2) % n, which is (rot+mid) % n. Hope it helps!

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        pivot = 0
        for i in range(1, len(nums)):
            if nums[i] <= nums[i - 1]:
                pivot = i
                break
        return self.bfs(0, len(nums) - 1, nums, target, pivot)
        
    def bfs(self, left, right, nums, target, pivot):
        while left <= right:
            mid = (left + right) // 2 
            rmid = (pivot + mid) % len(nums)
            if nums[rmid] < target:
                left = mid + 1
            elif nums[rmid] > target:
                right = mid - 1
            else:
                return rmid
        return -1

```

One more thing, we should use binary search to find the rotate point also. **the rotate point, is also the smallest value.**  [**https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/**](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)****

**But this is kinda complex idea**

The crux of the problem is that the lowest index will lie in the unsorted range or nums\[0] if whole array is sorted

```python
while low < high:
    mid = low + (high-low)//2
    if nums[low] > nums[mid]:
    # If we are here, it means that the range low to mid is unsorted, so we will discard the right part
        high = mid
    else:
    # If we are here, it means the range low to mid is sorted, so the min element will be nums[low] or in the range mid to high
        if nums[mid] < nums[high]:
            # If we are here, it means the right part is also sorted. So now the left part is also sorted and right part is also sorted, So nums[low] will be the lowest
            return nums[low]
        else:
            # If the right part is not sorted, so the min will lie in that range (the unsorted range)
            low = mid + 1
return nums[low]
```

or concisely:

```python
        left, right = 0, len(nums) - 1 #we need the right so
        while left < right: # <= lead to endless loop
            mid = (left + right) // 2
            if nums[mid] > nums[right]: 
            # if min is in right un sorted part
            # move the left until we reach the sorted part
                left = mid + 1
            else: # no, the min is in the sorted part. 
            #we simply shrink the right to reach the leftmost min
                right = mid
```

the if else code is very standard binary search. The only one changed is the condtion: `if nums[mid] > nums[right]` instead of `if nums[mid] < target`
