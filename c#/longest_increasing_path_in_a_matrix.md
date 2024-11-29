# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
public class Solution {
    private readonly int[][] directions = { new[] {0, 1}, new[] {1, 0}, new[] {0, -1}, new[] {-1, 0} }; // 4 possible directions: right, down, left, up

    public int LongestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.Length == 0 || matrix[0].Length == 0) return 0;
        int m = matrix.Length, n = matrix[0].Length;
        int[][] memo = new int[m][];
        for (int i = 0; i < m; i++) {
            memo[i] = new int[n];
        }
        int longestPath = 0;

        // Explore each cell in the matrix using DFS
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                longestPath = Math.Max(longestPath, Dfs(matrix, i, j, memo));
            }
        }

        return longestPath;
    }

    private int Dfs(int[][] matrix, int i, int j, int[][] memo) {
        if (memo[i][j] != 0) return memo[i][j]; // Return cached result if already computed

        int maxPath = 1; // Minimum path length is 1 (the cell itself)

        // Explore all four directions
        foreach (var dir in directions) {
            int x = i + dir[0], y = j + dir[1];
            if (x >= 0 && x < matrix.Length && y >= 0 && y < matrix[0].Length && matrix[x][y] > matrix[i][j]) {
                int length = 1 + Dfs(matrix, x, y, memo);
                maxPath = Math.Max(maxPath, length);
            }
        }

        memo[i][j] = maxPath; // Cache the result before returning

        return maxPath;
    }
}
```

## Approach 2: Topological Sort with Indegree

### Solution
csharp
```csharp
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
using System.Collections.Generic;

public class Solution {
    private readonly int[][] directions = { new[] {0, 1}, new[] {1, 0}, new[] {0, -1}, new[] {-1, 0} };

    public int LongestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.Length == 0 || matrix[0].Length == 0) return 0;
        int m = matrix.Length, n = matrix[0].Length;
        int[][] indegree = new int[m][];
        for (int i = 0; i < m; i++) {
            indegree[i] = new int[n];
        }

        // Calculate indegrees of each cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                foreach (var dir in directions) {
                    int x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                        indegree[x][y]++;
                    }
                }
            }
        }

        Queue<int[]> queue = new Queue<int[]>();
        // Initialize queue with cells having 0 indegree
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (indegree[i][j] == 0) {
                    queue.Enqueue(new[] {i, j});
                }
            }
        }

        int pathLength = 0;
        // Process each cell in the queue
        while (queue.Count > 0) {
            pathLength++;
            int size = queue.Count;
            for (int k = 0; k < size; k++) {
                int[] cell = queue.Dequeue();
                foreach (var dir in directions) {
                    int x = cell[0] + dir[0], y = cell[1] + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[cell[0]][cell[1]]) {
                        indegree[x][y]--;
                        if (indegree[x][y] == 0) {
                            queue.Enqueue(new[] {x, y});
                        }
                    }
                }
            }
        }

        return pathLength;
    }
}
```

