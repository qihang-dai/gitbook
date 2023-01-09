# Product of Array Except Self

Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [1,2,3,4]
</strong><strong>Output: [24,12,8,6]
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [-1,1,0,-3,3]
</strong><strong>Output: [0,0,9,0,0]
</strong></code></pre>



## Solution

Its clear, prefix and suffix. mutliple the left side and right side. done.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:

        l = len(nums)
        prefix = [0] * l
        suffix = [0] * l

        cursum = 1
        for i in range(l):
            cursum = nums[i] * cursum
            prefix[i] = cursum

        cursum = 1
        for i in range(l - 1, -1, -1):
            cursum = nums[i] * cursum
            suffix[i] = cursum
        
        # print(prefix, suffix)
        res = [0] * l

        res[0] = suffix[1]
        res[l - 1] = prefix[l - 2]
        for i in range(1, l - 1):
            res[i] = prefix[i - 1] * suffix[i + 1]
        
        return res
        
```

However for leetcode solution, we can use variable or write on the res array to save the space of prefix and suffix array. we can have res\[i] be the left side sum first, then loop nums\[i] and add right side sum

```java
public class Solution {
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] res = new int[n];
    res[0] = 1; // only have right side, so it will multiply the right side by one
    for (int i = 1; i < n; i++) {
        res[i] = res[i - 1] * nums[i - 1]; // each res[i] is the left side sum
    }
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        res[i] *= right; //res[n-1] only have the left side, 
                        //so right and left all start from one
        right *= nums[i]; //calculate the right side
    }
    return res;
}
```
