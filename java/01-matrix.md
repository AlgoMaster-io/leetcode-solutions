
### [Leetcode 542: 01 Matrix](https://leetcode.com/problems/01-matrix/)

#### Approaches:
1. [Breadth-First Search (BFS) from Zeros](#bfs-from-zeros)
2. [Dynamic Programming Approach](#dynamic-programming)

---

### Approach 1: [Breadth-First Search (BFS) from Zeros](#breadth-first-search)

**Intuition:**
- The problem requires finding the shortest distance from each cell containing `1` to the nearest cell containing `0`.
- A natural way to explore distances outward from source points (0's) is via a BFS, as BFS by nature explores equally in all directions level-by-level.
- Initialize the distance to `0` for all cells containing `0`.
- Enqueue all `0` cells initially and start the BFS.
- For each dequeued cell, update its neighboring cells if a shorter distance is discovered.

**Steps:**
1. Initialize a queue structure and set distances according to the input matrix — zero for `0` cells, infinity for `1` cells.
2. Add all zeros to a queue.
3. Perform a BFS where each cell updates its neighbors with its distance plus one, provided the neighbor's current value is greater than the newly proposed distance.
4. Continue until processing for all cells is complete.

```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int rows = mat.length;
        int cols = mat[0].length;
        int[][] dist = new int[rows][cols];
        Queue<int[]> queue = new LinkedList<>();

        // Initialize the distances and add all zero cells to the queue
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (mat[i][j] == 0) {
                    dist[i][j] = 0;
                    queue.add(new int[]{i, j});
                } else {
                    dist[i][j] = Integer.MAX_VALUE;
                }
            }
        }

        // Define the directions for exploring adjacent cells (up, down, left, right)
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        // Perform BFS
        while (!queue.isEmpty()) {
            int[] cell = queue.poll();
            int i = cell[0];
            int j = cell[1];

            // Explore all four directions
            for (int[] dir : directions) {
                int r = i + dir[0];
                int c = j + dir[1];

                // Check bounds and if we can update the distance
                if (r >= 0 && r < rows && c >= 0 && c < cols) {
                    if (dist[r][c] > dist[i][j] + 1) {
                        dist[r][c] = dist[i][j] + 1;
                        queue.add(new int[]{r, c});
                    }
                }
            }
        }

        return dist;
    }
}
```

**Time Complexity:** \(O(m \times n)\), where `m` and `n` are the matrix dimensions. Each cell is processed at most once.

**Space Complexity:** \(O(m \times n)\), needed for the distance matrix and the BFS queue.

---

### Approach 2: [Dynamic Programming Approach](#dynamic-programming)

**Intuition:**
- The idea is to use DP to find optimal substructure — checking paths dynamically based on previously computed paths.
- Since BFS from each zero can be suboptimal and cumbersome for large inputs, consider updating the distances recursively based on dynamic programming.
- Traverse the matrix two times: once from top-left to bottom-right, and again from bottom-right to top-left, to ensure that the shortest path from any direction is captured.

**Steps:**
1. Initialize the distance array with infinite places for `1` initially.
2. Traverse the matrix from the top-left, filling out shortest distances leveraging known zero positions and dynamic table values.
3. Traverse again from the bottom-right, updating the table further to reflect any shorter distances by considering the bottom-right-to-top-left direction.

```java
public class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int rows = mat.length;
        int cols = mat[0].length;
        int[][] dist = new int[rows][cols];
        
        // Initialize for "worst case" distances
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                dist[i][j] = (mat[i][j] == 0) ? 0 : Integer.MAX_VALUE;
            }
        }
        
        // Top-left to bottom-right
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (mat[i][j] != 0) {
                    // Check top
                    if (i > 0) {
                        dist[i][j] = Math.min(dist[i][j], dist[i - 1][j] + 1);
                    }
                    // Check left
                    if (j > 0) {
                        dist[i][j] = Math.min(dist[i][j], dist[i][j - 1] + 1);
                    }
                }
            }
        }
        
        // Bottom-right to top-left
        for (int i = rows - 1; i >= 0; i--) {
            for (int j = cols - 1; j >= 0; j--) {
                if (mat[i][j] != 0) {
                    // Check bottom
                    if (i < rows - 1) {
                        dist[i][j] = Math.min(dist[i][j], dist[i + 1][j] + 1);
                    }
                    // Check right
                    if (j < cols - 1) {
                        dist[i][j] = Math.min(dist[i][j], dist[i][j + 1] + 1);
                    }
                }
            }
        }
        
        return dist;
    }
}
```

**Time Complexity:** \(O(m \times n)\), the matrix is traversed twice.

**Space Complexity:** \(O(1)\), only a fixed amount of extra space used, modifying the input.

---

Both approaches efficiently compute the minimum distances while ensuring a dependable and scalable solution for large matrices. Each employs different core methodologies — BFS is intuitive but more involved, while DP leverages table array simplicity.


