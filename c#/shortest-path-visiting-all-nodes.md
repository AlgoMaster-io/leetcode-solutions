# [Leetcode 847: Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

### Solutions:

1. [Approach 1: Breadth-First Search (BFS) with Bit Masking](#approach-1)
2. [Approach 2: Dynamic Programming with Bit Masking (Optimized)](#approach-2)

---

## Approach 1: Breadth-First Search (BFS) with Bit Masking

In this approach, we use a BFS strategy with a queue to explore paths and bitmasks to efficiently track visited nodes. At each step, we attempt to visit unvisited nodes while updating our path mask. This allows us to explore all possible paths and find the shortest one.

### Intuition:

- Use BFS to explore all nodes starting from each node.
- Represent the state with `(node, visited_mask)`, where `visited_mask` is a bitmask representing visited nodes.
- The bitmask is used to keep track of visited nodes efficiently.
- The queue will help us explore all possibilities while ensuring the shortest path due to its FIFO nature.

### Time Complexity:
- **O((2^n) * n^2)** where n is the number of nodes. We consider all possible states, and each state loops through all adjacent nodes.

### Space Complexity:
- **O(2^n * n)** for the queue and the visited states.

### Implementation:

```csharp
using System.Collections.Generic;

public class Solution {
    public int ShortestPathLength(int[][] graph) {
        int n = graph.Length;
        int allVisited = (1 << n) - 1; // A bitmask where all n bits are set to 1
        var queue = new Queue<(int, int, int)>(); // (currentNode, visitedMask, pathLength)
        var visited = new HashSet<(int, int)>();  // Used to track visited states (node, visitedMask)

        // Start BFS from each node
        for (int i = 0; i < n; i++) {
            int startMask = 1 << i;  // Only the ith node is visited
            queue.Enqueue((i, startMask, 0));
            visited.Add((i, startMask));
        }

        // BFS loop
        while (queue.Count > 0) {
            var (current, mask, length) = queue.Dequeue();

            // Check if all nodes have been visited
            if (mask == allVisited) {
                return length;
            }

            // Explore all neighbors
            foreach (int neighbor in graph[current]) {
                int newMask = mask | (1 << neighbor);

                if (!visited.Contains((neighbor, newMask))) {
                    queue.Enqueue((neighbor, newMask, length + 1));
                    visited.Add((neighbor, newMask));
                }
            }
        }

        return -1; // This line should never be reached
    }
}
```

---

## Approach 2: Dynamic Programming with Bit Masking (Optimized)

This approach optimizes the BFS solution by using dynamic programming to reuse calculated costs and reduce redundant calculations.

### Intuition:

- Use a dp array to store the minimum path length to visit all nodes ending at a specific node.
- Transition from one state to another using previously computed states.

### Time Complexity:
- **O((2^n) * n^2)**, similar reasoning to the BFS, but optimized using dynamic programming.

### Space Complexity:
- **O(2^n * n)** for dp storage.

### Implementation:

```csharp
using System;

public class Solution {
    public int ShortestPathLength(int[][] graph) {
        int n = graph.Length;
        var dp = new int[1 << n, n];
        for (int mask = 0; mask < 1 << n; ++mask) {
            Array.Fill(dp[mask], Int32.MaxValue / 2);
        }

        for (int i = 0; i < n; ++i) {
            dp[1 << i, i] = 0;
        }

        // Populating dp array
        for (int mask = 0; mask < 1 << n; ++mask) {
            for (int u = 0; u < n; ++u) {
                if ((mask & (1 << u)) == 0) continue;
                foreach (int v in graph[u]) {
                    dp[mask | (1 << v), v] = Math.Min(dp[mask | (1 << v), v], dp[mask, u] + 1);
                }
            }
        }

        // Finding the minimum path length of visiting all nodes
        int minPath = Int32.MaxValue;
        for (int i = 0; i < n; ++i) {
            minPath = Math.Min(minPath, dp[(1 << n) - 1, i]);
        }
        return minPath;
    }
}
```

This dynamic programming solution improves upon BFS by reducing the number of state explorations through reuse, thus making it more efficient, particularly for larger graphs.

