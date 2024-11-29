# 1584. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approach 1: Prim's Algorithm 

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
import heapq

class Solution:
    def minCostConnectPoints(self, points):
        n = len(points)
        inMST = [False] * n
        minHeap = [(0, 0)]  # (cost, point)

        totalCost = 0
        edgesUsed = 0

        while edgesUsed < n:
            cost, u = heapq.heappop(minHeap)

            if inMST[u]:
                continue  # Skip if already included in the MST

            inMST[u] = True
            totalCost += cost
            edgesUsed += 1

            for v in range(n):
                if not inMST[v]:
                    dist = abs(points[u][0] - points[v][0]) + abs(points[u][1] - points[v][1])
                    heapq.heappush(minHeap, (dist, v))

        return totalCost
```

## Approach 2: Kruskal's Algorithm with Union-Find

### Solution
python
```python
# Time Complexity: O(n^2 log n)
# Space Complexity: O(n^2)
import heapq

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]

    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)

        if rootX != rootY:
            if self.rank[rootX] > self.rank[rootY]:
                self.parent[rootY] = rootX
            elif self.rank[rootX] < self.rank[rootY]:
                self.parent[rootX] = rootY
            else:
                self.parent[rootY] = rootX
                self.rank[rootX] += 1
            return True
        return False

class Solution:
    def minCostConnectPoints(self, points):
        n = len(points)
        edges = []

        # Calculate all possible edges and their costs
        for i in range(n):
            for j in range(i + 1, n):
                cost = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
                edges.append((cost, i, j))

        edges.sort()  # Sort edges by ascending cost

        uf = UnionFind(n)
        totalCost = 0
        edgesUsed = 0

        for cost, u, v in edges:
            if edgesUsed == n - 1:
                break

            if uf.union(u, v):
                totalCost += cost
                edgesUsed += 1

        return totalCost
```

