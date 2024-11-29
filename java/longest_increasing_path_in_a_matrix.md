# 329. [Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approach 1: DFS with Memoization

### Solution
```java
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
public class Solution {
    private int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}}; // 4 possible directions: right, down, left, up

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
        int m = matrix.length, n = matrix[0].length;
        int[][] memo = new int[m][n]; // Memoization array to store results of subproblems
        int longestPath = 0;

        // Explore each cell in the matrix using DFS
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                longestPath = Math.max(longestPath, dfs(matrix, i, j, memo));
            }
        }

        return longestPath;
    }

    private int dfs(int[][] matrix, int i, int j, int[][] memo) {
        if (memo[i][j] != 0) return memo[i][j]; // Return cached result if already computed

        int maxPath = 1; // Minimum path length is 1 (the cell itself)

        // Explore all four directions
        for (int[] dir : directions) {
            int x = i + dir[0], y = j + dir[1];
            if (x >= 0 && x < matrix.length && y >= 0 && y < matrix[0].length && matrix[x][y] > matrix[i][j]) {
                int length = 1 + dfs(matrix, x, y, memo);
                maxPath = Math.max(maxPath, length);
            }
        }

        memo[i][j] = maxPath; // Cache the result before returning

        return maxPath;
    }
}
```

## Approach 2: Topological Sort with Indegree

### Solution
```java
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    private int[][] directions = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

    public int longestIncreasingPath(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) return 0;
        int m = matrix.length, n = matrix[0].length;
        int[][] indegree = new int[m][n]; // Array to store number of incoming edges

        // Calculate indegrees of each cell
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int[] dir : directions) {
                    int x = i + dir[0], y = j + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[i][j]) {
                        indegree[x][y]++;
                    }
                }
            }
        }

        Queue<int[]> queue = new LinkedList<>();
        // Initialize queue with cells having 0 indegree
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (indegree[i][j] == 0) {
                    queue.offer(new int[]{i, j});
                }
            }
        }

        int pathLength = 0;
        // Process each cell in the queue
        while (!queue.isEmpty()) {
            int size = queue.size();
            pathLength++;
            for (int k = 0; k < size; k++) {
                int[] cell = queue.poll();
                for (int[] dir : directions) {
                    int x = cell[0] + dir[0], y = cell[1] + dir[1];
                    if (x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[cell[0]][cell[1]]) {
                        indegree[x][y]--;
                        if (indegree[x][y] == 0) {
                            queue.offer(new int[]{x, y});
                        }
                    }
                }
            }
        }

        return pathLength;
    }
}
```

