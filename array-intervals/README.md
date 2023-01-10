# Array, Intervals

{% embed url="https://leetcode.com/problems/insert-interval/" %}

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

<pre><code><strong>Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
</strong><strong>Output: [[1,5],[6,9]]
</strong></code></pre>

## Brutal Force Idea

3 condition:

1. the new interval has no overlapping
2. has overlapping with one side
3. has overlapping with both side

### pop out idea

We can not reuse the old array cause there maybe overlapping and merging, which means delete some element. so we think we can reconstruct it.

some idea is to record the start and end at extra array:

start: \[1, 6]

end: \[3, 9]

after insert:

start: \[1, 2, 6]

end: \[3, 5, 9]

seems not helping, how can we find the overlapping from this?

again the pop out idea seems not helpful. lets do the brutal force again

### vanilla brutal force

remeber, the array is non overlapping and sort by start. what we care is the insert start and end

we loop through the array, if we find the insert start is larger than one start but smaller than another end, we stop there.

then from where we stop, we loop through the end, if we find the the insert end is bigger than one start but smaller than one end, we stop there.&#x20;

on the way we record the min(start) and max(END), index at merge start and index at merge end, pick the front and end, and merge the three together into a new array.

we can pick up the front and end on the go.&#x20;

```python
class Solution:
    def insert(self, intervals, newInterval):
        res, n = [], newInterval
        for index, i in enumerate(intervals):
            if i[1] < n[0]:
                res.append(i)
            elif n[1] < i[0]:
                res.append(n)
                return res+intervals[index:]  # can return earlier
            else:  # overlap case, we rewrite the n, and it will get back to case2
                n[0] = min(n[0], i[0])
                n[1] = max(n[1], i[1])
        res.append(n)
        return res
```

However this is not straight forward and hard to think. instead i prefer this code: cut the insert into 3 cases: left, merge part, and right. then problem solved easily

```java
public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
    List<Interval> result = new LinkedList<>();
    int i = 0;
    // add all the intervals ending before newInterval starts
    while (i < intervals.size() && intervals.get(i).end < newInterval.start)
        result.add(intervals.get(i++));
    // merge all overlapping intervals to one considering newInterval
    while (i < intervals.size() && intervals.get(i).start <= newInterval.end) {
        newInterval = new Interval( // we could mutate newInterval here also
                Math.min(newInterval.start, intervals.get(i).start),
                Math.max(newInterval.end, intervals.get(i).end));
        i++;
    }
    result.add(newInterval); // add the union of intervals we got
    // add all the rest
    while (i < intervals.size()) result.add(intervals.get(i++)); 
    return result;
}
```

## [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

A bit different, but we need to detect the merged part rather than directly get it from the insert one.

still, we can append the original first, and compare it to the last list, if overlap, we rewrite the last list.

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

