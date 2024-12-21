# [Pacific Atlantic Water Flow - LeetCode](https://leetcode.com/problems/pacific-atlantic-water-flow/)

## Table of Contents:
1. [Brute Force Approach](#brute-force-approach)
2. [DFS Approach](#dfs-approach)

## Brute Force Approach

### Intuition:
The problem asks us to determine which cells have the ability to flow into both the Pacific and Atlantic ocean. A naive approach is to perform checks for every cell to determine if there exists a path to both oceans. We start from every cell and explore all possible paths using BFS while keeping track of height conditions.

### Steps:
1. Initialize two matrices to track which cells connect to the Pacific and Atlantic oceans.
2. For each cell, perform BFS to attempt to reach both oceans.
3. If a cell can reach both oceans, add it to the result set.

### Code:
```csharp
public class Solution {
    public IList<IList<int>> PacificAtlantic(int[][] matrix) {
        IList<IList<int>> result = new List<IList<int>>();
        int rows = matrix.Length, cols = rows > 0 ? matrix[0].Length : 0;
        if (rows == 0 || cols == 0) return result;

        // Initialize matrices to track ocean connectivity
        bool[,] pacific = new bool[rows, cols];
        bool[,] atlantic = new bool[rows, cols];

        // Explore all cells and see if they can reach both oceans
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (CanReachOcean(matrix, pacific, r, c, true) && CanReachOcean(matrix, atlantic, r, c, false)) {
                    result.Add(new List<int> { r, c });
                }
            }
        }
        return result;
    }

    private bool CanReachOcean(int[][] matrix, bool[,] ocean, int r, int c, bool isPacific) {
        Queue<(int, int)> queue = new Queue<(int, int)>();
        queue.Enqueue((r, c));
        int rows = matrix.Length, cols = matrix[0].Length;
        bool[,] visited = new bool[rows, cols];

        // Direction vectors for traversing up, down, left and right
        int[] dirR = { -1, 1, 0, 0 };
        int[] dirC = { 0, 0, -1, 1 };

        while (queue.Count > 0) {
            (int currentR, int currentC) = queue.Dequeue();
            if (isPacific && (currentR == 0 || currentC == 0)) return true;
            if (!isPacific && (currentR == rows - 1 || currentC == cols - 1)) return true;
            
            for (int d = 0; d < 4; d++) {
                int newR = currentR + dirR[d];
                int newC = currentC + dirC[d];

                if (newR >= 0 && newR < rows && newC >= 0 && newC < cols && 
                    !visited[newR, newC] && matrix[newR][newC] <= matrix[currentR][currentC]) {
                    visited[newR, newC] = true;
                    queue.Enqueue((newR, newC));
                }
            }
        }
        return false;
    }
}
```

### Time and Space Complexity:
- **Time Complexity:** \(O((m \times n)^2)\), as for each cell, a BFS is executed.
- **Space Complexity:** \(O(m \times n)\) used for the visited matrices.

## DFS Approach

### Intuition:
We improve the brute force approach using DFS traversal. The key idea here is to start from the ocean borders and work backward to determine which cells can flow into each ocean. This approach avoids recalculation by maintaining state through boolean matrices in place of space-consuming path explorations.

### Steps:
1. Initialize two matrices to track reachability from the Pacific and Atlantic oceans.
2. Perform DFS from every border cell of the Pacific and Atlantic.
3. After the traversal completes, identify cells marked true in both matrices.

### Code:
```csharp
public class Solution {
    public IList<IList<int>> PacificAtlantic(int[][] matrix) {
        IList<IList<int>> result = new List<IList<int>>();
        if (matrix == null || matrix.Length == 0 || matrix[0].Length == 0) return result;

        int rows = matrix.Length, cols = matrix[0].Length;
        bool[,] pacificReachable = new bool[rows, cols];
        bool[,] atlanticReachable = new bool[rows, cols];

        for (int i = 0; i < rows; i++) {
            DFS(matrix, pacificReachable, i, 0);
            DFS(matrix, atlanticReachable, i, cols - 1);
        }
        for (int i = 0; i < cols; i++) {
            DFS(matrix, pacificReachable, 0, i);
            DFS(matrix, atlanticReachable, rows - 1, i);
        }

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (pacificReachable[i, j] && atlanticReachable[i, j]) {
                    result.Add(new List<int> { i, j });
                }
            }
        }

        return result;
    }

    private void DFS(int[][] matrix, bool[,] reachable, int r, int c) {
        reachable[r, c] = true;

        // Direction vectors for traversing up, down, left and right
        int[] dirR = { -1, 1, 0, 0 };
        int[] dirC = { 0, 0, -1, 1 };
        int rows = matrix.Length, cols = matrix[0].Length;

        for (int d = 0; d < 4; d++) {
            int newR = r + dirR[d];
            int newC = c + dirC[d];

            // Ensure the new cell is within bounds, not already visited, and has a sufficient height to flow
            if (newR >= 0 && newR < rows && newC >= 0 && newC < cols &&
                !reachable[newR, newC] && matrix[newR][newC] >= matrix[r][c]) {
                DFS(matrix, reachable, newR, newC);
            }
        }
    }
}
```

### Time and Space Complexity:
- **Time Complexity:** \(O(m \times n)\), because each cell is visited once during DFS.
- **Space Complexity:** \(O(m \times n)\) for the reachability matrices.

This optimized approach efficiently calculates accessibility from both oceans using DFS and neatly categorizes the cells that meet the conditions.

