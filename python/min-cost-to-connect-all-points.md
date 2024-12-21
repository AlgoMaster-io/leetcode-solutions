# [1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approaches:
- [Approach 1: Prim's Algorithm (Brute Force with Min-Heap)](#approach-1-prims-algorithm-brute-force-with-min-heap)
- [Approach 2: Kruskal's Algorithm with Union-Find](#approach-2-kruskals-algorithm-with-union-find)

### Approach 1: Prim's Algorithm (Brute Force with Min-Heap)

**Intuition:**

Prim's Algorithm is a classical algorithm used for finding the Minimum Spanning Tree (MST) of a graph. The essence is to maintain a set of vertices that are already included in the MST and repeatedly add the smallest edge connecting the MST to the remaining vertices.

Steps:
1. Choose an arbitrary vertex to start, mark it visited.
2. Add all edges from this vertex to a min-heap based on their costs.
3. Choose the edge with the minimum cost that connects two vertices (one inside MST, one outside).
4. Continue this process until all vertices are included in the MST.

This approach makes use of a priority queue (min-heap) to efficiently fetch the smallest edge at each iteration.

```python
import heapq

def minCostConnectPoints(points):
    # Number of points
    n = len(points)

    # Priority queue (min-heap)
    minHeap = [(0, 0)] # Start with the 0th point with cost 0
    total_cost = 0
    added_vertices = set()

    while len(added_vertices) < n:
        cost, u = heapq.heappop(minHeap)

        if u in added_vertices:
            continue

        # Add point u to the MST
        total_cost += cost
        added_vertices.add(u)

        # Traverse all points to add new edges
        for v in range(n):
            if v not in added_vertices:
                # Calculate the distance (weight) between point u and point v
                distance = abs(points[u][0] - points[v][0]) + abs(points[u][1] - points[v][1])
                heapq.heappush(minHeap, (distance, v))

    return total_cost

# Time Complexity: O(N^2 log N), where N is the number of points. We traverse all possible pairs and maintain a min-heap.
# Space Complexity: O(N^2), due to the storage of edges in the heap.
```

### Approach 2: Kruskal's Algorithm with Union-Find

**Intuition:**

Kruskal’s Algorithm involves sorting all the edges of the graph and adding them one by one in order of increasing weight to the growing forest (initially containing no edges) provided adding the edge doesn’t form a cycle. Union-Find (Disjoint Set Union) is used to manage the vertices in the growing forest.

Steps:
1. Create all edges with their weights (Manhattan distance between points).
2. Sort all the edges based on their weights.
3. Initialize a Union-Find data structure.
4. Iterate over sorted edges and include an edge only if it doesn’t form a cycle.

```python
class UnionFind:
    def __init__(self, size):
        self.root = [i for i in range(size)]
        self.rank = [1] * size

    def find(self, x):
        if x != self.root[x]:
            self.root[x] = self.find(self.root[x])
        return self.root[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.root[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.root[rootX] = rootY
            else:
                self.root[rootY] = rootX
                self.rank[rootX] += 1
            return True
        return False

def minCostConnectPoints(points):
    n = len(points)
    edges = []

    # Generate all possible edges with their weights
    for i in range(n):
        for j in range(i + 1, n):
            weight = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
            edges.append((weight, i, j))

    # Sort edges based on weight
    edges.sort()

    # Initialize union-find structure
    uf = UnionFind(n)
    total_cost = 0
    for weight, u, v in edges:
        if uf.union(u, v):
            total_cost += weight

    return total_cost

# Time Complexity: O(N^2 log N), as there are O(N^2) edges to sort.
# Space Complexity: O(N^2), since we store all possible edges.
```

In conclusion, both Prim's and Kruskal's algorithms are efficient ways to solve this problem, with Kruskal's being arguably more intuitive for those familiar with Disjoint Set Union structures. The presence of spatial constraints will guide the choice between them in application-specific scenarios.

