# Matrix

## 01Matrix

Given an `m x n` binary matrix `mat`, return _the distance of the nearest_ `0` _for each cell_.

The distance between two adjacent cells is `1`.



## Solution

### Dfs

Look at this code, is this correct?

```python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        m, n = len(mat), len(mat[0])
        def bfs(i, j):
            if mat[i][j] == 0:
                return 0
            if i < 0 or j < 0 or i >= m or j >= n:
                return 0 #wrong
            shortest = min(bfs(i - 1, j), bfs(i + 1, j)) #wrong
            return shortest + 1
        

        res = [[0] * len(mat) for i in range(len(mat[0]))]
        for i in range(m):
            for j in range(n):
                if mat[i][j] == 1:
                    res[i][j] = bfs(i, j)
        return res

```

No.  I also found its hard to have a clear mind of dfs. **if you cant have a clear mind, very likely you are going in the wrong direciton**

1. its not bfs. its dfs. recursively
2. the return value when reach boundary is not right
3. you move to i - 1 then i - 1 move to i - 1 + 1, fall into infinite loop

So use bfs:

Note: List is a Python’s built-in data structure that can be used as a queue. Instead of enqueue() and dequeue(), append() and pop() function is used. However, lists are quite slow for this purpose because inserting or deleting an element at the beginning requires shifting all of the other elements by one, requiring O(n) time.

### BFS

Use deque() in python. pop means pop the last in (right most), popleft() is use as queue

```python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        q = deque()
        m, n = len(mat), len(mat[0])
        for i in range(m):
            for j in range(n):
                if mat[i][j] == 0:
                    q.append((i, j))
                else:
                    mat[i][j] = -1 # to do inplace change
        
        # print(mat)
        dir = [0, 1, 0, -1, 0]
        while q:
            r, c = q.popleft()
            for i in range(4):
                nr, nc = r + dir[i], c + dir[i + 1]
                if nr < 0 or nc < 0 or nr >= m or nc >= n or mat[nr][nc] != -1:
                    continue
                mat[nr][nc] = mat[r][c] + 1
                q.append((nr, nc))
                
        return mat
```

Be careful it rewrites the mat to -1 since it want to do inplace change, the original valur 1 we dont want it interfer with the distance 1, so we set it to -1

### DP

yes, adjacent cell has relationship, so dp.

since dfs goes all way out and jump backforth lead to endless recursion, we can do dp that is not endless recursion, we do it in two direction:

1. top down, dp\[i]\[j] = min(dp\[i + 1]\[j], dp\[i]\[j + 1])
2. left up, dp\[i]\[j] = min(dp\[i - 1]\[j], dp\[i]\[j - 1])

can reuse the mat i think.&#x20;

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```python
class Solution:  # 520 ms, faster than 96.50%
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        m, n = len(mat), len(mat[0])

        for r in range(m):
            for c in range(n):
                if mat[r][c] > 0:
                    top = mat[r - 1][c] if r > 0 else math.inf
                    left = mat[r][c - 1] if c > 0 else math.inf
                    mat[r][c] = min(top, left) + 1

        for r in range(m - 1, -1, -1):
            for c in range(n - 1, -1, -1):
                if mat[r][c] > 0:
                    bottom = mat[r + 1][c] if r < m - 1 else math.inf
                    right = mat[r][c + 1] if c < n - 1 else math.inf
                    mat[r][c] = min(mat[r][c], bottom + 1, right + 1)

        return mat
```

watch out in the second loop, the mat\[r]\[c] is inside the min(). This is because we need to take the first loop's result into consideration, compare it with bottom + 1 and right + 1.
