# [Leetcode 785: Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

## Approaches
1. [Depth-First Search (DFS) Approach - Color Two Coloring](#method-1-dfs)
2. [Breadth-First Search (BFS) Approach - Color Two Coloring](#method-2-bfs)

---

## Method 1: DFS

### Intuition

The goal is to determine if the given graph is bipartite. A graph is bipartite if the set of its vertices can be divided into two disjoint and independent sets U and V such that every edge connects a vertex in U to one in V. In simpler terms, you should be able to color the graph using two colors without two adjacent nodes having the same color.

DFS is used to color nodes in a graph. We use two colors, `0` and `1`, to differentiate the two sets of a bipartite graph. If we manage to color the graph without any node sharing the same color as its neighbors, the graph is bipartite.

### Implementation

```python
def isBipartite(graph):
    def dfs(node, color):
        # Color the node
        colors[node] = color
        # Iterate through all neighbors
        for neighbor in graph[node]:
            if colors[neighbor] == -1:
                # Color the neighbor alternately (using 1 - color)
                if not dfs(neighbor, 1 - color):
                    return False
            elif colors[neighbor] == color:
                # If a neighbor is the same color, return false
                return False
        return True

    n = len(graph)
    colors = [-1] * n  # Array to store colors of each node; -1 means uncolored

    # Traverse each node in the graph
    for i in range(n):
        if colors[i] == -1:
            if not dfs(i, 0):  # Start coloring with color 0
                return False
    return True
```

### Complexity Analysis
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. We might visit all vertices and edges in the graph once.
- **Space Complexity:** O(V), for the `colors` list and call stack in case of complete depth.

---

## Method 2: BFS

### Intuition

The BFS approach is similar to DFS, but instead of using a stack (recursion stack), we explicitly use a queue to explore nodes level by level. This helps to avoid deep recursion, which can sometimes be beneficial to prevent stack overflow in deep recursive DFS.

### Implementation

```python
from collections import deque

def isBipartite(graph):
    n = len(graph)
    colors = [-1] * n 

    for i in range(n):
        if colors[i] == -1:
            queue = deque([i])
            colors[i] = 0  # Start with color 0

            while queue:
                node = queue.popleft()
                for neighbor in graph[node]:
                    if colors[neighbor] == -1:
                        # Color with opposite color to current node
                        colors[neighbor] = 1 - colors[node]
                        queue.append(neighbor)
                    elif colors[neighbor] == colors[node]:
                        # If neighbor has the same color, the graph is not bipartite
                        return False
    return True
```

### Complexity Analysis
- **Time Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. We traverse through each node and edge once.
- **Space Complexity:** O(V), for the `colors` list and the queue data structure.

Both approaches utilize a two-color technique to ensure that adjacent nodes are colored differently. By carefully checking each node's neighbors during traversal, we verify the bipartite nature of the graph.

