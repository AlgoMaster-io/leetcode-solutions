# 684. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approach 1: Union-Find Algorithm

### Solution
python
```python
# Time Complexity: O(n * α(n)), where α is the inverse Ackermann function
# Space Complexity: O(n)

class Solution:
    def findRedundantConnection(self, edges):
        n = len(edges)
        parent = list(range(n + 1))
        rank = [1] * (n + 1)

        def find(node):
            # Path compression optimization
            if parent[node] != node:
                parent[node] = find(parent[node])
            return parent[node]

        def union(u, v):
            rootU = find(u)
            rootV = find(v)

            if rootU == rootV:
                return True

            # Union by rank
            if rank[rootU] > rank[rootV]:
                parent[rootV] = rootU
            elif rank[rootU] < rank[rootV]:
                parent[rootU] = rootV
            else:
                parent[rootV] = rootU
                rank[rootU] += 1
            
            return False

        for edge in edges:
            if union(edge[0], edge[1]):
                return edge

        return []  # If no redundant connection is found
```

## Approach 2: DFS to Detect Cycle

### Solution
python
```python
# Time Complexity: O(n^2), in the worst case for a dense graph
# Space Complexity: O(n)

from collections import defaultdict

class Solution:
    def findRedundantConnection(self, edges):
        n = len(edges)
        graph = defaultdict(list)

        def hasCycleDFS(source, target, visited):
            if visited[source]:
                return False
            
            visited[source] = True

            for neighbor in graph[source]:
                if neighbor == target:
                    continue

                if visited[neighbor] or hasCycleDFS(neighbor, target, visited):
                    return True

            return False

        for edge in edges:
            if hasCycleDFS(edge[0], edge[1], [False] * (n + 1)):
                return edge
            graph[edge[0]].append(edge[1])
            graph[edge[1]].append(edge[0])

        return []  # If no redundant connection is found
```


