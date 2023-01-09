# maximum subarray

## Brutal Force:

### prefix

1. calculate the prefix sum, loop every start and end to get the middle subarray value.
2. we can calculate the prefix max. this method is the simlar idea as the dp and greedy: we keep the prefix if its useful, if not we use our own single nums\[i]

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        premax = [*nums]
        premax[0] = nums[0]
        for i in range(1, len(nums)):
            premax[i] = max(premax[i], premax[i - 1] + nums[i])
        res = max(premax)
        return res
```

### on the go brutal froce

two loop, start from every position and calculate the max on the go for every ending. no need for prefix sum.

## Dynamic programming ways

<pre><code><strong>Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
</strong><strong>Output: 6
</strong><strong>Explanation: [4,-1,2,1] has the largest sum = 6.
</strong></code></pre>

My first thought was wrong. I didnt think about what the dp means and what the equation should be. All i thought is get something out of memory and double hands pray.

the code that comes out of my head:

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0] * len(nums)
        print(dp)
        dp[0] = nums[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i - 1] + nums[i], dp[i - 1])
        return dp[-1]

```

think carefully, this would output the all the positive value in the array with nums\[0], which is totally nonsense.

so what should we code here?

1. we need dp\[i] = max(dp\[i - 1] + nums\[i], ???)
2. dp\[i] should means maximum subarray value ending at nums\[i], thus we can add the nums\[i] if it is positive. contains nums\[i] or not? if we want dp\[-1] to be the result it shouldnt. however since we need consecutive subarray, we want nums\[i] here so it can be consecutive. thus dp\[-1] is not the result but the max subarray with ending and nums\[-1]. we would need a res to store the max
3. if dp\[i - 1] is positive , regardless of nums\[i], we should add it to get bigger. but if its negative, we should neglect it, use nums\[i]
4. thus the new equaiton is : dp\[i] = max(dp\[i - 1] + nums\[i],  nums\[i])

what to initialize here? ofc dp\[0] = nums\[0]. the rest doesnt matter cause we would calculate it out.

the solution:

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [0] * len(nums)
        print(dp)
        dp[0] = nums[0]
        res = nums[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i - 1] + nums[i], nums[i])
            res = max(dp[i], res)
        return res

#these two are same
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [*nums]
        res = nums[0]
        for i in range(1, len(nums)):
            dp[i] = max(dp[i - 1] + nums[i], dp[i])
            res = max(dp[i], res)
        return res
```

If we want dp\[-1] to be the result, we can somehow do a two dimensional dp that dp\[0]\[i] to store the maxiumum till now with or without nums\[i]. but the core is the same, just use the dp\[i] = max(dp\[i - 1] + nums\[i],  nums\[i])

## Greedy algo

The problem should be simple with greedy algo: we want consective. if the consective is positive we keep it, if not we reset the sum to the current nums\[i]

watch out for when to calculate the res. we reset it for the next nums\[i]

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        cursum = 0
        res = nums[0]
        for n in nums:
            cursum += n
            res = max(cursum, res)
            if cursum < 0: #reset for next calculation
                cursum = 0
        return res

#these two are same algo, some subtle code skills
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        cursum = nums[0]
        res = nums[0]
        for i in range(1, len(nums)):
            if cursum < 0:
                cursum = nums[i]
            else:
                cursum += nums[i]
            res = max(res, cursum)
        return res
```

Or, we can get rid of the variable cursum, but rewrite the array instead. This is Kadane algorithm

```python
for i in range(1, len(nums)):
        if nums[i-1] > 0:
            nums[i] += nums[i-1]
    return max(nums)
```

## Divide and Conquer:

not interested, but it utilize the logN time of divide or binary. see [link](https://leetcode.com/problems/maximum-subarray/solutions/1595195/c-python-7-simple-solutions-w-explanation-brute-force-dp-kadane-divide-conquer/?orderBy=most\_votes)
