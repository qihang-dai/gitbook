# Intervals

## Merge Intervals

56\. Merge IntervalsMedium17.6K611Companies

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

&#x20;

**Example 1:**

<pre><code><strong>Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
</strong><strong>Output: [[1,6],[8,10],[15,18]]
</strong><strong>Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: intervals = [[1,4],[4,5]]
</strong><strong>Output: [[1,5]]
</strong><strong>Explanation: Intervals [1,4] and [4,5] are considered overlapping.
</strong></code></pre>

## Solution

Sort the array by the start so we can compare it. We give it one side in order, then we can make the other side ordered too.

One thing to notice is :&#x20;

```
for i in range(10):
    if i == 5:
        i += 3
    print(i)
```

This will not skip the loop. still printing 0 1 2 3 4 8 6 7 8 9. Thats a Python thing for double loop

I wrote code like this at first, which does not work

```
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals = sorted(intervals, key = lambda x: x[0])
        res = []
        i = 0
        while i < len(intervals):
            left, right = intervals[i]
            while i < len(intervals) - 1 and intervals[i][1] >= intervals[i + 1][0]: 
                right = max(intervals[i + 1][-1], intervals[i][-1])
                i += 1
                print(i)
            res.append([left, right])
            i += 1
        return res
        
```

turns out we need to do a crash and remake. other intervals have to compare with the new merges interval. the code above still compare the old intervals without taking the new merged one in to consideration. 消消乐.

Here are two type of correct code:

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals = sorted(intervals, key = lambda x : x[0])
        res = []
        for i in intervals:
            if res and res[-1][1] >= i[0]:
                res[-1][1] = max(res[-1][1], i[1])
            else:
                res.append(i)
        return res
```

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals = sorted(intervals, key = lambda x : x[0])
        res = []
        for i in intervals:
            if res and res[-1][1] >= i[0]:
                res[-1][1] = max(res[-1][1], i[1])
            else:
                res.append(i)
        return res
```
