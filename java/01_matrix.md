# 542. [01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approach: Breadth-First Search (BFS)

### Solution
```java
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and distance matrix
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int rows = mat.length;
        int cols = mat[0].length;
        int[][] distances = new int[rows][cols];
        boolean[][] visited = new boolean[rows][cols];
        Queue<int[]> queue = new LinkedList<>();

        // Step 1: Initialize the queue with all '0' cells and mark them as visited
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (mat[i][j] == 0) {
                    queue.add(new int[] { i, j });
                    visited[i][j] = true;
                }
            }
        }

        // Step 2: Perform BFS to calculate distances
        int[][] directions = { { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } };
        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            for (int[] dir : directions) {
                int newRow = current[0] + dir[0];
                int newCol = current[1] + dir[1];

                // Check bounds and if the cell is not visited
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && !visited[newRow][newCol]) {
                    distances[newRow][newCol] = distances[current[0]][current[1]] + 1;
                    queue.add(new int[] { newRow, newCol });
                    visited[newRow][newCol] = true;
                }
            }
        }

        return distances;
    }
}
```