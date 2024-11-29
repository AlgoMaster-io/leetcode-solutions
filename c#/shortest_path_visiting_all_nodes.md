# 847. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approach 1: Breadth-First Search (BFS) with Bitmask

### Solution
csharp
```csharp
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

using System;
using System.Collections.Generic;

public class Solution {
    public int ShortestPathLength(int[][] graph) {
        int n = graph.Length;
        if (n == 1) return 0;

        // Queue for BFS, storing {node, visited nodes mask}
        Queue<(int, int)> queue = new Queue<(int, int)>();
        bool[,] visited = new bool[n, 1 << n];

        // Initialize queue and visited set with all starting nodes
        for (int i = 0; i < n; ++i) {
            queue.Enqueue((i, 1 << i));
            visited[i, 1 << i] = true;
        }

        int steps = 0;
        while (queue.Count > 0) {
            int size = queue.Count;
            for (int i = 0; i < size; ++i) {
                var (node, mask) = queue.Dequeue();

                // If all nodes are visited
                if (mask == (1 << n) - 1) return steps;

                // Visit all the neighbors
                foreach (int nei in graph[node]) {
                    int nextMask = mask | (1 << nei);
                    if (!visited[nei, nextMask]) {
                        queue.Enqueue((nei, nextMask));
                        visited[nei, nextMask] = true;
                    }
                }
            }
            steps++;
        }
        return -1;
    }
}
```

## Approach 2: Dynamic Programming with Bitmask

### Solution
csharp
```csharp
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

using System;
using System.Collections.Generic;

public class Solution {
    public int ShortestPathLength(int[][] graph) {
        int n = graph.Length;
        int[,] dp = new int[1 << n, n];
        for (int i = 0; i < (1 << n); ++i) {
            for (int j = 0; j < n; ++j) {
                dp[i, j] = int.MaxValue;
            }
        }

        Queue<(int, int)> queue = new Queue<(int, int)>();
        for (int i = 0; i < n; ++i) {
            queue.Enqueue((1 << i, i));
            dp[1 << i, i] = 0;
        }

        while (queue.Count > 0) {
            var (mask, node) = queue.Dequeue();

            foreach (int nei in graph[node]) {
                int nextMask = mask | (1 << nei);
                if (dp[nextMask, nei] > dp[mask, node] + 1) {
                    dp[nextMask, nei] = dp[mask, node] + 1;
                    queue.Enqueue((nextMask, nei));
                }
            }
        }

        int result = int.MaxValue;
        for (int i = 0; i < n; ++i) {
            result = Math.Min(result, dp[(1 << n) - 1, i]);
        }
        return result;
    }
}
```

Both approaches use BFS and dynamic programming with bitmask to efficiently find the shortest path to visit all nodes, ensuring optimality in solving the problem.

