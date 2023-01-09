# K element

{% embed url="https://leetcode.com/problems/k-closest-points-to-origin/solutions/220235/java-three-solutions-to-this-classical-k-th-problem/?orderBy=most_votes" %}
Great leetcode solution&#x20;
{% endembed %}

I solved it myself, but by the most intuitive and brutal idea: sort based on distance with key lambda, and pick the top k.

### Sort, Nlog(N)

```python
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        ka = [0] * len(points)
        for i in range(len(points)):
            x = points[i][0]
            y = points[i][1]
            dis = (x ** 2 + y ** 2) ** 0.5
            ka[i] = (dis, points[i])
        # print(ka)
        sortedka = sorted(ka, key = lambda x : x[0])
        # print(sortedka)
        res = [x[1] for x in sortedka]
        return res[:k]
```

turns out this sort is redundant and not short enough. we dont really need to put the distane inside the array, but just use it to sort.

```
    public int[][] kClosest(int[][] points, int K) {
        Arrays.sort(points, Comparator.comparing(p -> p[0] * p[0] + p[1] * p[1]));
        return Arrays.copyOfRange(points, 0, K);
    }
```

## Heap

Not familiar with heap in python so i write some note here:

* **heappush(heap, ele)**: This function is used to insert the element mentioned in its arguments into a heap. The **** order is adjusted, so that heap structure is maintained.
* **heappop(heap)**: This function is used to remove and return the smallest element from the heap. The order is adjusted, so that heap structure is maintained.
* **heappushpop(heap, ele)**:- This function combines the functioning of both push and pop operations in one statement, increasing efficiency. Heap order is maintained after this operation.
* **heapreplace(heap, ele)**:- This function also inserts and pops elements in one statement, but it is different from the above function. In this, the element is first popped, then the element is pushed. i.e, the value larger than the pushed value can be returned. heapreplace() returns the smallest value originally in the heap regardless of the pushed element as opposed to heappushpop().

```python
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        
        heap = []
        
        for (x, y) in points:
            dist = -(x*x + y*y)
            if len(heap) == K:
                heapq.heappushpop(heap, (dist, x, y))
            else:
                heapq.heappush(heap, (dist, x, y))
        
        return [(x,y) for (dist,x, y) in heap]
```

#### Find the largest and smallest elements from Heap in Python

* **nlargest(k, iterable, key = fun)**: This function is used to return the k largest elements from the iterable specified and satisfy the key if mentioned.
* **nsmallest(k, iterable, key = fun)**: This function is used to return the k smallest elements from the iterable specified and satisfy the key if mentioned.

```
    def kClosest(self, points, K):
        return heapq.nsmallest(K, points, lambda (x, y): x * x + y * y)
```

Heap is good for streaming data. After learning database and understand how large the data is in actual world, we should use heap rather than sort everything. &#x20;

## **Quick select**

**Quick sort has some quirks, so i dont want to go too deep here.**
