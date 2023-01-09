# Clone Graph



Given a reference of a node in a [**connected**](https://en.wikipedia.org/wiki/Connectivity\_\(graph\_theory\)#Connected\_graph) undirected graph.

Return a [**deep copy**](https://en.wikipedia.org/wiki/Object\_copying#Deep\_copy) (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.

the node.val is the node's index (1,2,3,4)

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

The graph is stored in adjacent list

## Solution

I know we should use dfs or bfs. But i dont know if we use bfs we still need an hashmap to store the visited or created list.

I looked at the solution.&#x20;

1. we do bfs on the old graph, create the deepcopy of the root node, as well as its neighbors
2. we append the old graph child whatever to the queue
3. we pop out another old node. is that not created? we create one and put in map. we pick the new clone from the map. Then we look at the old neighbors and construct new. if not created we created it
4. we pop out another old node. is that not created?  yes cause we already created it in the neighbors. so we dont need to check this one, but only to check the one created in neighbors
5. and append the one we get from the map it to the neighbors.

## Leetcode solution

it never create the new node in the neighbors, but create

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        if not node:
            return None
        q = deque([node])
        created = {node.val: Node(node.val, [])}
        print(created)
        while q:
            cur = q.popleft()
            newcur = created[cur.val]
            for n in cur.neighbors:
                if n.val not in created:
                    nn = Node(n.val, [])
                    created[n.val] = nn
                    q.append(n)
                newcur.neighbors.append(created[n.val])
        return created[node.val]
        
```
