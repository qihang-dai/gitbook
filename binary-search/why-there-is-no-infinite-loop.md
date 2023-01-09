# Why there is no infinite loop?

standard binary search code:

```python
left, right = 0, len(nums)
while left < right:
    mid = (left + right) // 2
    if nums[mid] < target:
        left = mid + 1
    else:
        right = mid
```

so the search space, \[left...right] shrink over loops.&#x20;

when there would be infinite loops: left and right remian unchanged.

here left would always be add 1, so it will change

for right = mid, since mid =(left + right) // 2, if left + 1 = right, mid would be the value of left, which is right - 1, thus right also shirnk change.

And at last, when left == right, only one element left, we break out.

