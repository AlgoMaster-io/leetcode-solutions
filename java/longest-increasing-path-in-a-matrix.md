# [Leetcode 329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Table of Contents
1. [Approach 1: Depth-First Search (DFS) with Memoization](#approach-1)
2. [Approach 2: Topological Sorting (Kahn's Algorithm)](#approach-2)

### Approach 1: Depth-First Search (DFS) with Memoization

#### Intuition:
The problem can be solved using a depth-first search approach with memoization. For each cell, we perform a DFS to find the longest increasing path starting from that cell. To avoid recomputing the result for a cell that has already been computed, we use memoization to store the result of the longest path for each cell.

#### Java Code:
```java
public class Solution {
    // Directions for moving up, down, left, and right.
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }

        int rows = matrix.length, cols = matrix[0].length;
        int[][] memo = new int[rows][cols];
        int maxPath = 0;

        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                maxPath = Math.max(maxPath, dfs(matrix, r, c, memo));
            }
        }
        
        return maxPath;
    }

    private int dfs(int[][] matrix, int r, int c, int[][] memo) {
        // If already computed, retrieve the memoized result
        if (memo[r][c] != 0) {
            return memo[r][c];
        }
        
        int maxLength = 1; // The path length is at least 1 starting from this cell.

        // Explore the four possible directions
        for (int[] dir : DIRECTIONS) {
            int x = r + dir[0], y = c + dir[1];
            if (x >= 0 && x < matrix.length && y >= 0 && y < matrix[0].length && matrix[x][y] > matrix[r][c]) {
                maxLength = Math.max(maxLength, 1 + dfs(matrix, x, y, memo));
            }
        }
        
        // Memoize and return the longest increasing path from current cell.
        memo[r][c] = maxLength;
        return maxLength;
    }
}
```

#### Time Complexity:
- **O(m * n):** We explore each cell once, memoizing the results, thus preventing redundant computations.

#### Space Complexity:
- **O(m * n):** The space due to recursive stack and memoization table.


### Approach 2: Topological Sorting (Kahn's Algorithm)

#### Intuition:
Imagine the matrix as a directed graph where an edge points from node `u` to node `v` if `v` can be reached from `u` (i.e., `v` is greater than `u`). The nodes have a direction based on increasing values. We can utilize Kahn's algorithm for topological sorting to iterate layers of increasing values starting from zero in-degree nodes.

#### Java Code:
```java
import java.util.*;

public class Solution {
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;

        int rows = matrix.length, cols = matrix[0].length;
        int[][] inDegrees = new int[rows][cols];
       
        // Calculate in-degrees
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                for (int[] dir : DIRECTIONS) {
                    int x = r + dir[0], y = c + dir[1];
                    if (x >= 0 && x < rows && y >= 0 && y < cols && matrix[x][y] > matrix[r][c]) {
                        inDegrees[x][y]++;
                    }
                }
            }
        }
        
        // Queue for nodes with in-degree zero
        Queue<int[]> queue = new LinkedList<>();
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (inDegrees[r][c] == 0) {
                    queue.offer(new int[]{r, c});
                }
            }
        }
        
        int maxLength = 0;
        
        while (!queue.isEmpty()) {
            maxLength++;
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                int[] cell = queue.poll();
                for (int[] dir : DIRECTIONS) {
                    int x = cell[0] + dir[0], y = cell[1] + dir[1];
                    if (x >= 0 && x < rows && y >= 0 && y < cols && matrix[x][y] > matrix[cell[0]][cell[1]]) {
                        inDegrees[x][y]--;
                        if (inDegrees[x][y] == 0) {
                            queue.offer(new int[]{x, y});
                        }
                    }
                }
            }
        }
        
        return maxLength;
    }
}
```

#### Time Complexity:
- **O(m * n):** We process each cell several times as we decrement in-degrees and enqueue new nodes.

#### Space Complexity:
- **O(m * n):** The space used by the in-degree table and queue.

These solutions work efficiently for large matrices due to optimal memorization and level-based processing using topological sorting techniques.

