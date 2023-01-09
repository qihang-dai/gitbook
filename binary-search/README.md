# Binary Search

## [find-first-and-last-position-of-element-in-sorted-array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/submissions/825474310/)

There are many type of binary search, and many variation of binary search. One key understanding is that the left boundary and right boundary have to move until convergence. Thats why we move `left = mid + 1 right = mid` rather than `right = mid - 1  left = mid` when doing `while left < right` cause mid = (left + right) / 2 would be the lower floor value and may result in endless loop. So its when to move left or when to move right thing.

Several thing notable about binary search:

#### 1. if val not found, the left/right boundary would shrink to the left (the least smaller) or right (the least larger)?&#x20;

Imagine seatch for 8 when the array is 1, 3, 6, 9, 10. &#x20;

once midval = 6, smaller, then move one forward, it would be 9 always bigger than the targeted value 9. Thus afterwards would always be move right inner, and the result is the right least value of the targeted value. Thats why in python bisect is equal to bisect\_right (acc bisect\_right is removed)\
\
Bisect\_left or Right, if not found since left = mid + 1, would always get to the right extra one

#### 2. find first == bisect\_left, find last == bisect\_right - 1

To find first or last occurence, we dont return the val when its found. instead,&#x20;

When the val is found:&#x20;

**bisect left or right, the key diff is which side pointer to move when the val(med) == target**

No need to change much about the original binary search template&#x20;

If we move left again, it will move to the least larger one (right bound).&#x20;

If the val is found at the mid and we move the right to the mid again, it will push the mid back to the left, the we get the left most occurence.

### Bisect Codes

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
    
        def bisect_left(target):
            l, h = 0, len(nums)
            while l < h:
                mid = (l + h) // 2
                if nums[mid] < target:
                    l = mid + 1
                else:
                    h = mid
            return h
        
        def bisect_right(target):
            l, h = 0, len(nums)
            while l < h:
                mid = (l + h) // 2
                if nums[mid] <= target:
                    l = mid + 1
                else:
                    h = mid
            return h
        
        
        res = [bisect_left(target), bisect_right(target) - 1]
        left, right = res
        if left >= right + 1:
            return [-1 ,-1]
        else:
            return res
```

Similar question:

{% embed url="https://leetcode.com/problems/first-bad-version/" %}
Its just a Bisect\_left
{% endembed %}

