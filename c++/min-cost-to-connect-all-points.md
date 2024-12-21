# [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Table of Contents
- [Approach 1: Prim's Algorithm (using Min-Heap)](#approach-1-prims-algorithm-using-min-heap)
- [Approach 2: Kruskal's Algorithm (using Union-Find)](#approach-2-kruskals-algorithm-using-union-find)

---

## Approach 1: Prim's Algorithm (using Min-Heap)

### Intuition:
The Prim's algorithm is a classic approach to find the Minimum Spanning Tree (MST) from a graph. In this problem, we're given a set of points in a 2D plane, which can be considered as complete graph edges forming between all points. The goal is to connect all points with minimum total cost, which is equivalent to finding an MST.

1. Start from any arbitrary point, and grow the MST by adding the smallest edge that connects a point in the MST with a point outside.
2. Use a priority queue (min-heap) to efficiently get the smallest edge.

### Steps:
1. Choose any arbitrary starting point.
2. Use a min-heap to keep track of the minimum cost edge to extend the MST.
3. For each point, add its neighbors to the min-heap.
4. Keep adding edges to the MST until all points are included.

### Time and Space Complexity:
- **Time Complexity:** O(N^2 log N), where N is the number of points. Each insertion to or extraction from the heap takes O(log N) time, and we potentially handle N^2 edges.
- **Space Complexity:** O(N^2) for storing all possible edges.

### Code:
```cpp
#include <vector>
#include <queue>
#include <cmath>

using namespace std;

int minCostConnectPoints(vector<vector<int>>& points) {
    int n = points.size();
    if (n == 0) return 0;

    // Min-heap to store {cost, pointIndex}
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<>> minHeap;
    vector<bool> inMST(n, false); // Check if the point is already in the MST

    minHeap.push({0, 0}); // Starting from the first point
    int totalCost = 0;
    int edgesUsed = 0;

    while (edgesUsed < n && !minHeap.empty()) {
        auto [cost, u] = minHeap.top();
        minHeap.pop();

        if (inMST[u]) continue; // If already in MST, skip

        // Include this point in MST
        inMST[u] = true;
        totalCost += cost;
        edgesUsed++;

        // Check all points to find the next minimum edge
        for (int v = 0; v < n; ++v) {
            if (!inMST[v]) {
                int nextCost = abs(points[u][0] - points[v][0]) + abs(points[u][1] - points[v][1]);
                minHeap.push({nextCost, v});
            }
        }
    }

    return totalCost;
}
```

---

## Approach 2: Kruskal's Algorithm (using Union-Find)

### Intuition:
Kruskal's algorithm is another way to find the MST from a graph by sorting all edges and adding them one by one to the MST, using a union-find structure to avoid cycles.

1. Build all possible edges from the given points.
2. Sort edges by their weights (cost).
3. Use the Union-Find structure to add edges to the MST without forming any cycles.

### Steps:
1. Create all possible edges between points with their calculated costs.
2. Sort the edges in ascending order based on their cost.
3. Initialize Union-Find structure to manage point components.
4. Traverse the sorted edges, connecting components without forming cycles.

### Time and Space Complexity:
- **Time Complexity:** O(N^2 log N), dominated by sorting the edges where N is the number of points.
- **Space Complexity:** O(N^2) for storing all possible edges.

### Code:
```cpp
#include <vector>
#include <algorithm>
#include <cmath>

using namespace std;

class UnionFind {
public:
    UnionFind(int size) : parent(size), rank(size, 0) {
        for (int i = 0; i < size; ++i) {
            parent[i] = i;
        }
    }

    int find(int u) {
        if (parent[u] != u) {
            parent[u] = find(parent[u]); // Path compression
        }
        return parent[u];
    }

    bool unionSets(int u, int v) {
        int rootU = find(u);
        int rootV = find(v);
        if (rootU == rootV) return false; // Already connected

        // Union by rank
        if (rank[rootU] < rank[rootV]) {
            parent[rootU] = rootV;
        } else if (rank[rootU] > rank[rootV]) {
            parent[rootV] = rootU;
        } else {
            parent[rootV] = rootU;
            rank[rootU]++;
        }
        return true;
    }

private:
    vector<int> parent;
    vector<int> rank;
};

int minCostConnectPoints(vector<vector<int>>& points) {
    int n = points.size();
    if (n == 0) return 0;

    // Create all possible edges with their costs
    vector<pair<int, pair<int, int>>> edges; // {cost, {point1, point2}}
    for (int u = 0; u < n; ++u) {
        for (int v = u + 1; v < n; ++v) {
            int cost = abs(points[u][0] - points[v][0]) + abs(points[u][1] - points[v][1]);
            edges.push_back({cost, {u, v}});
        }
    }

    // Sort edges based on cost
    sort(edges.begin(), edges.end());

    // Initialize Union-Find
    UnionFind uf(n);
    int totalCost = 0;
    int edgesUsed = 0;

    for (const auto& [cost, nodes] : edges) {
        if (edgesUsed == n - 1) break; // If we have used n-1 edges, MST is complete
        int u = nodes.first;
        int v = nodes.second;
        if (uf.unionSets(u, v)) {
            totalCost += cost;
            edgesUsed++;
        }
    }

    return totalCost;
}
```


