# 1584. [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

## Approach 1: Prim's Algorithm 

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(n)
import java.util.PriorityQueue;
import java.util.Comparator;

public class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        boolean[] inMST = new boolean[n];
        PriorityQueue<int[]> minHeap = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        
        minHeap.offer(new int[]{0, 0}); // (cost, point)

        int totalCost = 0;
        int edgesUsed = 0;

        while (edgesUsed < n) {
            int[] top = minHeap.poll();
            int cost = top[0];
            int u = top[1];

            if (inMST[u]) continue; // Skip if already included in the MST

            inMST[u] = true;
            totalCost += cost;
            edgesUsed++;

            for (int v = 0; v < n; v++) {
                if (!inMST[v]) {
                    int dist = Math.abs(points[u][0] - points[v][0]) + Math.abs(points[u][1] - points[v][1]);
                    minHeap.offer(new int[]{dist, v});
                }
            }
        }

        return totalCost;
    }
}
```

## Approach 2: Kruskal's Algorithm with Union-Find

### Solution
```java
// Time Complexity: O(n^2 log n)
// Space Complexity: O(n^2)
import java.util.PriorityQueue;
import java.util.Comparator;
import java.util.Arrays;

class Solution {
    public int minCostConnectPoints(int[][] points) {
        int n = points.length;
        PriorityQueue<int[]> edges = new PriorityQueue<>(Comparator.comparingInt(a -> a[2]));

        // Calculate all possible edges and their costs
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int cost = Math.abs(points[i][0] - points[j][0]) + Math.abs(points[i][1] - points[j][1]);
                edges.offer(new int[]{i, j, cost});
            }
        }

        UnionFind uf = new UnionFind(n);
        int totalCost = 0;
        int edgesUsed = 0;

        while (edgesUsed < n - 1) {
            int[] edge = edges.poll();
            int u = edge[0];
            int v = edge[1];
            int cost = edge[2];

            if (uf.union(u, v)) {
                totalCost += cost;
                edgesUsed++;
            }
        }

        return totalCost;
    }

    private static class UnionFind {
        private final int[] parent;
        private final int[] rank;

        UnionFind(int n) {
            parent = new int[n];
            rank = new int[n];
            Arrays.fill(rank, 1);
            for (int i = 0; i < n; i++) parent[i] = i;
        }

        int find(int x) {
            if (parent[x] != x) {
                parent[x] = find(parent[x]); // Path compression
            }
            return parent[x];
        }

        boolean union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);

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

