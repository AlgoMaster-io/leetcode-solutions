# 847. [Shortest Path Visiting All Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes/)

## Approach 1: Breadth-First Search (BFS) with Bitmask

### Solution
```java
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

import java.util.*;

public class Solution {
    public int shortestPathLength(int[][] graph) {
        int n = graph.length;
        if (n == 1) return 0;

        // Queue for BFS, storing {node, visited nodes mask}
        Queue<int[]> queue = new LinkedList<>();
        boolean[][] visited = new boolean[n][1 << n];

        // Initialize queue and visited set with all starting nodes
        for (int i = 0; i < n; ++i) {
            queue.offer(new int[]{i, 1 << i});
            visited[i][1 << i] = true;
        }

        int steps = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; ++i) {
                int[] current = queue.poll(); 
                int node = current[0], mask = current[1];

                // If all nodes are visited
                if (mask == (1 << n) - 1) return steps;

                // Visit all the neighbors
                for (int nei : graph[node]) {
                    int nextMask = mask | (1 << nei);
                    if (!visited[nei][nextMask]) {
                        queue.offer(new int[]{nei, nextMask});
                        visited[nei][nextMask] = true;
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
```java
// Time Complexity: O(2^n * n^2)
// Space Complexity: O(2^n * n)

import java.util.*;

public class Solution {
    public int shortestPathLength(int[][] graph) {
        int n = graph.length;
        int[][] dp = new int[1 << n][n];
        for (int i = 0; i < (1 << n); ++i) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }

        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < n; ++i) {
            queue.offer(new int[]{1 << i, i});
            dp[1 << i][i] = 0;
        }

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int mask = current[0], node = current[1];

            for (int nei : graph[node]) {
                int nextMask = mask | (1 << nei);
                if (dp[nextMask][nei] > dp[mask][node] + 1) {
                    dp[nextMask][nei] = dp[mask][node] + 1;
                    queue.offer(new int[]{nextMask, nei});
                }
            }
        }

        int result = Integer.MAX_VALUE;
        for (int i = 0; i < n; ++i) {
            result = Math.min(result, dp[(1 << n) - 1][i]);
        }
        return result;
    }
}
```

Both approaches use BFS and dynamic programming with bitmask to efficiently find the shortest path to visit all nodes, ensuring optimality in solving the problem.

