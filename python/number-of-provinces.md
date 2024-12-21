# [Leetcode 547: Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

### Approaches:
- [Approach 1: Depth First Search (DFS)](#approach-1-depth-first-search-dfs)
- [Approach 2: Breadth First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Union-Find (Disjoint Set Union, DSU)](#approach-3-union-find)

---

## Approach 1: Depth First Search (DFS)

### Intuition:
Consider each city as a node in a graph, where an edge exists if one city is directly connected to another. A province then is a connected component in this graph. To find the number of provinces, we can use DFS to traverse each connected component of nodes (cities).

### Steps:
1. Initialize a visited list to track which cities have been visited.
2. Iterate over each city. If that city hasn't been visited, this indicates the start of a new province.
3. Use DFS to traverse all cities in the current province, marking them visited.
4. Increase the province count each time a new unvisited city is found.

```python
def findCircleNum(isConnected):
    def dfs(city):
        for j, is_connected in enumerate(isConnected[city]):
            if is_connected and not visited[j]:
                visited[j] = True
                dfs(j)

    n = len(isConnected)
    visited = [False] * n
    province_count = 0
    
    for i in range(n):
        if not visited[i]:
            dfs(i)
            province_count += 1

    return province_count
```

#### Time Complexity:
- O(n^2), where n is the number of cities. We potentially visit each city and check all its connections once.

#### Space Complexity:
- O(n) for the visited list. In the worst case, the recursion stack in DFS could also go to O(n).

---

## Approach 2: Breadth First Search (BFS)

### Intuition:
Similar to DFS, we can use BFS to explore each city and its connections to find all the cities within the same province.

### Steps:
1. Use a queue to facilitate BFS traversal.
2. For each unvisited city, start a BFS traversal marking all directly and indirectly connected cities as visited.
3. Count the number of BFS initiations which corresponds to the number of provinces.

```python
from collections import deque

def findCircleNum(isConnected):
    n = len(isConnected)
    visited = [False] * n
    province_count = 0
    
    for i in range(n):
        if not visited[i]:
            queue = deque([i])
            while queue:
                city = queue.popleft()
                visited[city] = True
                for j, is_connected in enumerate(isConnected[city]):
                    if is_connected and not visited[j]:
                        queue.append(j)
            province_count += 1

    return province_count
```

#### Time Complexity:
- O(n^2), where n is the number of cities. Similar to DFS, each city and its edges are checked once.

#### Space Complexity:
- O(n) for the visited list and queue.

---

## Approach 3: Union-Find (Disjoint Set Union, DSU)

### Intuition:
Union-Find is effective in handling connectivity queries. Treat each city like a node and connection as an edge to find connected components (provinces).

### Steps:
1. Initialize a parent array where each node is its own parent.
2. Define helper functions `find` and `union` to manage the root connections.
3. Iterate through the matrix and union directly connected cities.
4. Count the unique roots representing different provinces.

```python
def findCircleNum(isConnected):
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    def union(x, y):
        rootX = find(x)
        rootY = find(y)
        if rootX != rootY:
            parent[rootX] = rootY

    n = len(isConnected)
    parent = list(range(n))

    for i in range(n):
        for j in range(i + 1, n):
            if isConnected[i][j]:
                union(i, j)

    province_count = len({find(i) for i in range(n)})
    return province_count
```

#### Time Complexity:
- Almost O(n^2), because each union and find operation is nearly constant time thanks to path compression.

#### Space Complexity:
- O(n) for the parent array managing the sets.

