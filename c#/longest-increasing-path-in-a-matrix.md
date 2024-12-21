# [Leetcode 329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approaches:
1. [Depth-First Search (DFS) with Memoization](#approach-1-dfs-with-memoization)
2. [Topological Sort using Indegree (Dynamic Programming)](#approach-2-topological-sort-using-indegree)

---

## Approach 1: DFS with Memoization

### Intuition:

The problem can be approached using Depth-First Search (DFS) to explore all increasing paths starting from each cell in the matrix. Since computing these paths independently for each cell is inefficient (due to overlapping subproblems), employing memoization will optimize the process. We need to store the results of previously computed cells to prevent recomputation, ensuring the algorithm remains efficient.

### Implementation:

```csharp
public class Solution {
    private static int[][] directions = new int[][] {
        new int[] {1, 0},  // down
        new int[] {-1, 0}, // up
        new int[] {0, 1},  // right
        new int[] {0, -1}  // left
    };

    public int LongestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.Length == 0) return 0;

        int rows = matrix.Length;
        int cols = matrix[0].Length;
        // Cache for memoization
        int[,] memo = new int[rows, cols];
        int longestPath = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                int currentPathLength = DFS(matrix, r, c, memo);
                longestPath = Math.Max(longestPath, currentPathLength);
            }
        }

        return longestPath;
    }

    private int DFS(int[][] matrix, int r, int c, int[,] memo) {
        if (memo[r, c] != 0) return memo[r, c];
        
        int maxPathLength = 1; // Minimum length is 1 (the cell itself)

        foreach (var dir in directions) {
            int newR = r + dir[0];
            int newC = c + dir[1];
            
            // Check boundaries and ensure the path is increasing
            if (newR >= 0 && newR < matrix.Length && newC >= 0 && newC < matrix[0].Length &&
                matrix[newR][newC] > matrix[r][c]) {
                int pathLength = 1 + DFS(matrix, newR, newC, memo);
                maxPathLength = Math.Max(maxPathLength, pathLength);
            }
        }

        // Memoize the result
        memo[r, c] = maxPathLength;
        return maxPathLength;
    }
}
```

**Time Complexity:** O(m * n), where m is the number of rows and n is the number of columns. Each cell is visited once and each recursive call explores a maximum of four directions.

**Space Complexity:** O(m * n) due to the memoization table and the recursive call stack.

---

## Approach 2: Topological Sort using Indegree (Dynamic Programming)

### Intuition:

Another approach is to utilize topological sorting by viewing the grid cells as graph nodes with directed edges that point from smaller to larger values. The key insight is that the length of the longest path to a node is the longest path to any of its direct predecessors plus one. We can use indegree counting to simplify this computation with dynamic programming by iteratively removing leaves (nodes with zero indegree).

### Implementation:

```csharp
public class Solution {
    private static int[][] directions = new int[][] {
        new int[] {1, 0},
        new int[] {-1, 0},
        new int[] {0, 1},
        new int[] {0, -1}
    };

    public int LongestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.Length == 0) return 0;

        int rows = matrix.Length;
        int cols = matrix[0].Length;
        int[,] indegree = new int[rows, cols];
        Queue<(int, int)> queue = new Queue<(int, int)>();
        
        // Calculate indegree for each cell
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                foreach (var dir in directions) {
                    int newR = r + dir[0];
                    int newC = c + dir[1];
                    if (newR >= 0 && newR < rows && newC >= 0 && newC < cols &&
                        matrix[newR][newC] > matrix[r][c]) {
                        indegree[newR, newC]++;
                    }
                }
            }
        }

        // Initialize queue with cells of indegree zero
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (indegree[r, c] == 0) {
                    queue.Enqueue((r, c));
                }
            }
        }

        int longestPath = 0;

        // Process nodes in topological order
        while (queue.Count > 0) {
            longestPath++;
            int size = queue.Count;
            for (int i = 0; i < size; i++) {
                var (r, c) = queue.Dequeue();
                
                foreach (var dir in directions) {
                    int newR = r + dir[0];
                    int newC = c + dir[1];
                    if (newR >= 0 && newR < rows && newC >= 0 && newC < cols &&
                        matrix[newR][newC] > matrix[r][c]) {
                        indegree[newR, newC]--;
                        if (indegree[newR, newC] == 0) {
                            queue.Enqueue((newR, newC));
                        }
                    }
                }
            }
        }

        return longestPath;
    }
}
```
**Time Complexity:** O(m * n), since each cell and edge will be processed once.

**Space Complexity:** O(m * n) due to the use of the indegree matrix and queue for BFS traversal.

Each approach offers different perspectives on solving the problem, ensuring efficiency and understanding in graph traversal and dynamic programming techniques.

