# 1631. [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## Approach 1: Dijkstra's Algorithm with Priority Queue

### Solution
```java
// Import necessary libraries
import java.util.PriorityQueue;
import java.util.Arrays;

// Time Complexity: O(m * n * log(m * n)), where m is the number of rows and n is the number of columns.
// Space Complexity: O(m * n)
public class Solution {
    // Direction vectors for exploring adjacent cells (right, left, down, up)
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

    public int minimumEffortPath(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;

        // Min-heap priority queue to select the path with the minimum effort so far
        PriorityQueue<int[]> minHeap = new PriorityQueue<>((a, b) -> Integer.compare(a[2], b[2]));

        // Efforts matrix to track the minimum effort required to reach each cell
        int[][] efforts = new int[m][n];
        for (int[] row : efforts) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }
        efforts[0][0] = 0;

        // Start from the top-left corner of the grid
        minHeap.add(new int[]{0, 0, 0});

        while (!minHeap.isEmpty()) {
            // Extract the cell with the minimum effort
            int[] current = minHeap.poll();
            int x = current[0];
            int y = current[1];
            int currentEffort = current[2];

            // If we've reached the bottom-right corner
            if (x == m - 1 && y == n - 1) {
                return currentEffort;
            }

            // Explore all adjacent cells
            for (int[] direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                // Check bounds
                if (newX >= 0 && newX < m && newY >= 0 && newY < n) {
                    // Calculate the effort required to move to the adjacent cell
                    int effort = Math.max(currentEffort, Math.abs(heights[newX][newY] - heights[x][y]));

                    // If this path to the adjacent cell is better, update efforts and heap
                    if (effort < efforts[newX][newY]) {
                        efforts[newX][newY] = effort;
                        minHeap.add(new int[]{newX, newY, effort});
                    }
                }
            }
        }

        return 0; // Shouldn't reach here. Always possible to reach the end.
    }
}
```

## Approach 2: Binary Search + Depth First Search (DFS)

### Solution
```java
// Import necessary libraries
import java.util.LinkedList;
import java.util.Queue;

// Time Complexity: O(m * n * log(maxDifference))
// Space Complexity: O(m * n)
public class Solution {

    // Direction vectors for exploring adjacent cells (right, left, down, up)
    private static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
    public int minimumEffortPath(int[][] heights) {
        int m = heights.length;
        int n = heights[0].length;
        int left = 0, right = 1_000_000, result = 1_000_000;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (canReachEndWithEffort(heights, mid, m, n)) {
                result = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        return result;
    }

    // Helper function to determine if there is a path from (0, 0) to (m - 1, n - 1) under the given maxEffort
    private boolean canReachEndWithEffort(int[][] heights, int maxEffort, int m, int n) {
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0, 0});
        visited[0][0] = true;

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int x = current[0];
            int y = current[1];

            if (x == m - 1 && y == n - 1) {
                return true;
            }

            for (int[] direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                if (newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY]) {
                    int currentEffort = Math.abs(heights[newX][newY] - heights[x][y]);

                    if (currentEffort <= maxEffort) {
                        visited[newX][newY] = true;
                        queue.add(new int[]{newX, newY});
                    }
                }
            }
        }

        return false;
    }
}
```

