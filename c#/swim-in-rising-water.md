# [Leetcode 778: Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

## Table of Contents
- [Approach 1: Dijkstra's Algorithm Using a Priority Queue](#approach-1)
- [Approach 2: Binary Search over Time with DFS](#approach-2)
- [Approach 3: Union-Find Data Structure](#approach-3)

---

## Approach 1: Dijkstra's Algorithm Using a Priority Queue

### Intuition
The problem essentially involves finding the path with the minimum maximum height, from the top-left corner to the bottom-right corner. This can be efficiently achieved using a priority queue, which helps in exploring cells based on the water level at those cells, similar to Dijkstra's algorithm for shortest paths.

### Implementation

```csharp
using System.Collections.Generic;

public class Solution {
    public int SwimInWater(int[][] grid) {
        int n = grid.Length;
        // Priority queue to store cells with the water level
        PriorityQueue<(int height, int x, int y), int> pq = new PriorityQueue<(int height, int x, int y), int>();
        pq.Enqueue((grid[0][0], 0, 0), grid[0][0]);

        bool[,] visited = new bool[n, n];
        int[] directions = new int[] { -1, 0, 1, 0, -1 };
        int maxHeight = 0;
        
        while (pq.Count > 0) {
            var current = pq.Dequeue();
            int height = current.height;
            int x = current.x;
            int y = current.y;

            // Update the maximum height encountered
            maxHeight = Math.Max(maxHeight, height);

            // If reached the bottom-right corner
            if (x == n - 1 && y == n - 1) {
                return maxHeight;
            }

            if (visited[x, y]) continue;
            visited[x, y] = true;

            // Explore the neighboring cells
            for (int i = 0; i < 4; i++) {
                int nx = x + directions[i];
                int ny = y + directions[i + 1];

                if (nx >= 0 && nx < n && ny >= 0 && ny < n && !visited[nx, ny]) {
                    pq.Enqueue((grid[nx][ny], nx, ny), grid[nx][ny]);
                }
            }
        }

        // Just for function signature satisfaction, code should never reach here
        return -1;
    }
}
```

### Complexity Analysis
- **Time Complexity:** \(O(N^2 \log N)\), where \(N\) is the number of rows (or columns) in the grid. This is due to the use of a priority queue that processes each cell.
- **Space Complexity:** \(O(N^2)\) for the priority queue and visited array.

---

## Approach 2: Binary Search over Time with DFS

### Intuition
Another way to solve the problem is to use binary search over the range of possible times (water levels) and check if a certain water level allows a path from start to end using Depth First Search (DFS).

### Implementation

```csharp
public class Solution {
    public int SwimInWater(int[][] grid) {
        int n = grid.Length;
        int left = grid[0][0];
        int right = n * n - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;
            bool[,] visited = new bool[n, n];
            if (CanSwim(grid, mid, 0, 0, visited)) {
                right = mid; // Try for a lower water level
            } else {
                left = mid + 1; // Increase the water level
            }
        }
        return left;
    }

    private bool CanSwim(int[][] grid, int threshold, int x, int y, bool[,] visited) {
        int n = grid.Length;
        if (x < 0 || x >= n || y < 0 || y >= n || visited[x, y] || grid[x][y] > threshold) return false;
        if (x == n - 1 && y == n - 1) return true;
        
        visited[x, y] = true;
        return CanSwim(grid, threshold, x + 1, y, visited) ||
               CanSwim(grid, threshold, x - 1, y, visited) ||
               CanSwim(grid, threshold, x, y + 1, visited) ||
               CanSwim(grid, threshold, x, y - 1, visited);
    }
}
```

### Complexity Analysis
- **Time Complexity:** \(O(N^2 \log N)\), due to binary search over the water levels and DFS to check connectivity.
- **Space Complexity:** \(O(N^2)\) for the visited array used in DFS.

---

## Approach 3: Union-Find Data Structure

### Intuition
Utilizing a Union-Find data structure (also known as Disjoint Set Union, DSU) can be another approach. By iterating over a sorted list of locations based on height values, it's possible to union adjacent squares and check if the start and end are connected.

### Implementation

```csharp
public class Solution {
    public int SwimInWater(int[][] grid) {
        int n = grid.Length;
        UnionFind uf = new UnionFind(n * n);
        List<(int height, int x, int y)> heights = new List<(int height, int x, int y)>();
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                heights.Add((grid[i][j], i, j));
            }
        }

        heights.Sort((a, b) => a.height.CompareTo(b.height));

        int[] directions = new int[] { -1, 0, 1, 0, -1 };
        foreach (var item in heights) {
            int height = item.height;
            int x = item.x;
            int y = item.y;
            int idx = x * n + y;
            
            // Union adjacent cells that are already traversable
            for (int i = 0; i < 4; i++) {
                int nx = x + directions[i];
                int ny = y + directions[i + 1];
                int nIdx = nx * n + ny;

                if (nx >= 0 && nx < n && ny >= 0 && ny < n && grid[nx][ny] <= height) {
                    uf.Union(idx, nIdx);
                }
            }

            // Check if the start and end cells are connected
            if (uf.Connected(0, n * n - 1)) {
                return height;
            }
        }

        // Just for function signature satisfaction, should never reach here
        return -1;
    }

    class UnionFind {
        private int[] parent;
        private int[] rank;

        public UnionFind(int size) {
            parent = new int[size];
            rank = new int[size];
            for (int i = 0; i < size; i++) {
                parent[i] = i;
                rank[i] = 1;
            }
        }

        public int Find(int x) {
            if (parent[x] != x) {
                parent[x] = Find(parent[x]);
            }
            return parent[x];
        }

        public void Union(int x, int y) {
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
            }
        }

        public bool Connected(int x, int y) {
            return Find(x) == Find(y);
        }
    }
}
```

### Complexity Analysis
- **Time Complexity:** \(O(N^2 \log N)\), primarily because the heights list is sorted, and union-find operations are nearly constant amortized time.
- **Space Complexity:** \(O(N^2)\) for the union-find data structure.

---

These approaches offer different trade-offs in implementation and performance, allowing you to select one based on your constraints or preferences.

