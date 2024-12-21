# [Problem: Leetcode 684 - Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## Approaches
- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Union-Find (Disjoint Set Union)](#approach-2-union-find-disjoint-set-union)

---

## Approach 1: Depth First Search (DFS)

### Intuition:
The problem essentially asks us to identify the edge which, if removed, will make the graph a tree. A tree is an acyclic connected graph, so finding the cycle is key.

The basic idea with DFS is to attempt to build the graph edge by edge and determine if adding a new edge creates a cycle (i.e., if both vertices of the edge have been visited by the DFS originating from one of them).

### Steps:
1. Initialize an adjacency list for the graph.
2. Iterate over each edge and perform DFS to check if adding this edge creates a cycle.
3. If the cycle is detected, return this edge as the result.
4. Otherwise, keep extending the graph.

### Implementation:
```python
def findRedundantConnection(edges):
    def dfs(source, target, visited):
        if source == target:
            return True
        visited.add(source)
        for neighbor in graph[source]:
            if neighbor not in visited and dfs(neighbor, target, visited):
                return True
        return False

    graph = {}  # adjacency list
    for u, v in edges:
        # Before adding the edge, check for a cycle 
        visited = set()
        if u in graph and v in graph and dfs(u, v, visited):
            return [u, v]
        if u not in graph:
            graph[u] = []
        if v not in graph:
            graph[v] = []
        # Add the edge in the graph
        graph[u].append(v)
        graph[v].append(u)

# Graph is constructed incrementally, an edge that forms a cycle is returned
```

### Complexity Analysis:
- **Time Complexity**: O(N^2), where N is the number of vertices. This occurs because, in the worst case, we might perform DFS for each edge, checking all vertices.
- **Space Complexity**: O(N), used for storing the graph and visited set in the worst case.

---

## Approach 2: Union-Find (Disjoint Set Union)

### Intuition:
The Union-Find data structure helps efficiently manage a collection of disjoint sets and supports union (merge) and find (identify) operations. By using Union-Find, we can track the connected components of the graph to determine if an edge connects two vertices that are already part of the same component, indicating a cycle.

### Steps:
1. Each node starts in its own set initially.
2. For each edge, check if both nodes of the edge belong to the same set (i.e., they are connected already).
3. If they belong to the same set, this edge is redundant.
4. Otherwise, union the two sets.

### Implementation:
```python
def findRedundantConnection(edges):
    parent = list(range(len(edges) + 1))
    rank = [1] * (len(edges) + 1)

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            # Union by rank
            if rank[rootX] > rank[rootY]:
                parent[rootY] = rootX
            elif rank[rootX] < rank[rootY]:
                parent[rootX] = rootY
            else:
                parent[rootY] = rootX
                rank[rootX] += 1
            return True
        return False

    for u, v in edges:
        if not union(u, v):
            return [u, v]
```

### Complexity Analysis:
- **Time Complexity**: O(N * α(N)), where α(N) is the inverse Ackermann function, which is very slow-growing. Therefore, this solution is nearly O(N).
- **Space Complexity**: O(N), for storing the parent and rank arrays.

