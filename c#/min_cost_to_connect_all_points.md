# 1584. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approach 1: Prim's Algorithm 

### Solution
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int MinCostConnectPoints(int[][] points) {
        int n = points.Length;
        bool[] inMST = new bool[n];
        var minHeap = new PriorityQueue<int[], int>(Comparer<int>.Create((a, b) => a - b));
        
        minHeap.Enqueue(new int[]{0, 0}, 0); // (cost, point)

        int totalCost = 0;
        int edgesUsed = 0;

        while (edgesUsed < n) {
            var top = minHeap.Dequeue();
            int cost = top[0];
            int u = top[1];

            if (inMST[u]) continue; // Skip if already included in the MST

            inMST[u] = true;
            totalCost += cost;
            edgesUsed++;

            for (int v = 0; v < n; v++) {
                if (!inMST[v]) {
                    int dist = Math.Abs(points[u][0] - points[v][0]) + Math.Abs(points[u][1] - points[v][1]);
                    minHeap.Enqueue(new int[]{dist, v}, dist);
                }
            }
        }

        return totalCost;
    }
}
```

## Approach 2: Kruskal's Algorithm with Union-Find

### Solution
```csharp
// Time Complexity: O(n^2 log n)
// Space Complexity: O(n^2)
using System;
using System.Collections.Generic;

public class Solution {
    public int MinCostConnectPoints(int[][] points) {
        int n = points.Length;
        var edges = new PriorityQueue<int[], int>(Comparer<int>.Create((a, b) => a - b));

        // Calculate all possible edges and their costs
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int cost = Math.Abs(points[i][0] - points[j][0]) + Math.Abs(points[i][1] - points[j][1]);
                edges.Enqueue(new int[]{i, j, cost}, cost);
            }
        }

        UnionFind uf = new UnionFind(n);
        int totalCost = 0;
        int edgesUsed = 0;

        while (edgesUsed < n - 1) {
            var edge = edges.Dequeue();
            int u = edge[0];
            int v = edge[1];
            int cost = edge[2];

            if (uf.Union(u, v)) {
                totalCost += cost;
                edgesUsed++;
            }
        }

        return totalCost;
    }

    private class UnionFind {
        private readonly int[] parent;
        private readonly int[] rank;

        public UnionFind(int n) {
            parent = new int[n];
            rank = new int[n];
            Array.Fill(rank, 1);
            for (int i = 0; i < n; i++) parent[i] = i;
        }

        public int Find(int x) {
            if (parent[x] != x) {
                parent[x] = Find(parent[x]); // Path compression
            }
            return parent[x];
        }

        public bool Union(int x, int y) {
            int rootX = Find(x);
            int rootY = Find(y);

            if (rootX != rootY) {
                if (rank[rootX] > rank[rootY]) {
                    parent[rootY] = rootX;
                } else if (rank[rootX] < rank[rootY]) {
                    parent[rootX] = rootY;
                } else {
                    parent[rootY] = rootX;
                    rank[rootX]++;
                }
                return true;
            }

            return false;
        }
    }
}
```

