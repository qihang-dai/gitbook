# Topological Sort

## 207. Course ScheduleMedium12K471Companies

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

* For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return `true` if you can finish all courses. Otherwise, return `false`.

&#x20;

**Example 1:**

<pre><code><strong>Input: numCourses = 2, prerequisites = [[1,0]]
</strong><strong>Output: true
</strong><strong>Explanation: There are a total of 2 courses to take. 
</strong>To take course 1 you should have finished course 0. So it is possible.
</code></pre>

**Example 2:**

<pre><code><strong>Input: numCourses = 2, prerequisites = [[1,0],[0,1]]
</strong><strong>Output: false
</strong><strong>Explanation: There are a total of 2 courses to take. 
</strong>To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
</code></pre>

## Mysolution

I know its topo sort, add the zero in node into the queue. but i cant remeber clearly, so i get messed up about how to determine true or false, wether we need a visited hashset.

The idea is do top sort, remove zero node and reduce their child's in count. if the child count is zero, we can add it back to the queue cause its removable. '

Like BFS but not even like BFS. There are several types of BFS:

1. level traversing. we need split each level. where you need size = len(q) and for in range(size)
2. just traversing. we dont need loop through a certain level, we just do BFS and keep adding it into the queue.

**I found myself caught in endless debugging trap due to no clear thought, then i get up a little bit above the water, despite keep debuging and look at the test case, i try to think what idea got me wrong and make the code so complex. then i remeber my thoughts was wrong and i delete the visited hashset() and remember i should add only zero in node to the queue.**

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        count = defaultdict(int)
        graph = defaultdict(list)
        q = deque()
        for a, b in prerequisites:
            count[a] += 1
            graph[b].append(a)
        for k in graph:
            if count[k] == 0:
                q.append(k)
        
        while q:
            size = len(q)
            cur = q.popleft()
            for neib in graph[cur]:
                count[neib] -= 1
                if count[neib] == 0:
                    q.append(neib)
        for k, v in count.items():
            if v > 0:
                return False
        return True

```

## Leetcode Solution

Simpler python version from lee251. we can also use array instead of dictionary to store the count. Note we can also use array instead of dictionary to do adjacent list

```python
    def canFinish(self, n, prerequisites):
        G = [[] for i in range(n)]
        degree = [0] * n
        for j, i in prerequisites:
            G[i].append(j)
            degree[j] += 1
        bfs = [i for i in range(n) if degree[i] == 0]
        for i in bfs:
            for j in G[i]:
                degree[j] -= 1
                if degree[j] == 0:
                    bfs.append(j)
        return len(bfs) == n
```

Better for understanding java version. We can use matrix instead of adjacent list for graph, if its a dense graph.&#x20;

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    int[][] matrix = new int[numCourses][numCourses]; // i -> j
    int[] indegree = new int[numCourses];
    
    for (int i=0; i<prerequisites.length; i++) {
        int ready = prerequisites[i][0];
        int pre = prerequisites[i][1];
        if (matrix[pre][ready] == 0)
            indegree[ready]++; //duplicate case
        matrix[pre][ready] = 1;
    }
    
    int count = 0;
    Queue<Integer> queue = new LinkedList();
    for (int i=0; i<indegree.length; i++) {
        if (indegree[i] == 0) queue.offer(i);
    }
    while (!queue.isEmpty()) {
        int course = queue.poll();
        count++;
        for (int i=0; i<numCourses; i++) {
            if (matrix[course][i] != 0) {
                if (--indegree[i] == 0)
                    queue.offer(i);
            }
        }
    }
    return count == numCourses;
}
```
