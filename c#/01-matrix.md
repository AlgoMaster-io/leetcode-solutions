# [542. 01 Matrix](https://leetcode.com/problems/01-matrix/)

## Approaches
- [Approach 1: Breadth-First Search (BFS)](#approach-1-bfs)
- [Approach 2: Dynamic Programming](#approach-2-dp)
- [Approach 3: Optimized BFS with a 2-pass solution](#approach-3-optimized-bfs)

### Approach 1: Breadth-First Search (BFS)

**Intuition:**
  - The fundamental idea is to find the shortest path from each cell containing `1` to the nearest cell containing `0`.
  - We can consider each cell with `0` as the source node and perform a breadth-first search (BFS) to find the shortest distance to all `1`s.

**Steps:**
1. Initialize the result matrix with `int.MaxValue` for each cell.
2. Use a queue to track coordinates of `0` cells and begin BFS from here.
3. For each `0`, attempt to move in all four directions, updating any `1` cell's distance if a shorter path is discovered.

```csharp
public class Solution {
    public int[][] UpdateMatrix(int[][] matrix) {
        int rows = matrix.Length;
        int cols = matrix[0].Length;
        int[][] distances = new int[rows][];
        Queue<(int, int)> queue = new Queue<(int, int)>();

        for (int i = 0; i < rows; i++) {
            distances[i] = new int[cols];
            for (int j = 0; j < cols; j++) {
                // Initialize distances. 0 cells will be the start of BFS.
                if (matrix[i][j] == 0) {
                    queue.Enqueue((i,j));
                } else {
                    distances[i][j] = int.MaxValue;
                }
            }
        }

        int[][] directions = new int[][] { new int[] {1, 0}, new int[] {-1, 0}, new int[] {0, 1}, new int[] {0, -1} };

        while (queue.Count > 0) {
            (int x, int y) = queue.Dequeue();
            for (int i = 0; i < directions.Length; i++) {
                int newX = x + directions[i][0];
                int newY = y + directions[i][1];
                // Ensure within bounds and found a shorter path
                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols) {
                    if (distances[newX][newY] > distances[x][y] + 1) {
                        distances[newX][newY] = distances[x][y] + 1;
                        queue.Enqueue((newX, newY));
                    }
                }
            }
        }
        return distances;
    }
}
```

- **Time Complexity:** O(rows * cols), as each cell is processed at most once.
- **Space Complexity:** O(rows * cols), for the `distances` matrix and queue space.

### Approach 2: Dynamic Programming

**Intuition:**
  - We can reduce the problem into finding the minimum distance in two passes: one forward and one backward.
  - Update the current cell's distance based on its neighboring cells.

**Steps:**
1. Pass from top-left to bottom-right. For each cell update its distance using the top and left neighbors.
2. Pass from bottom-right to top-left. Update its distance using the bottom and right neighbors.

```csharp
public class Solution {
    public int[][] UpdateMatrix(int[][] matrix) {
        int rows = matrix.Length;
        int cols = matrix[0].Length;
        int[][] distances = new int[rows][];

        for (int i = 0; i < rows; i++) {
            distances[i] = new int[cols];
            for (int j = 0; j < cols; j++) {
                distances[i][j] = matrix[i][j] == 0 ? 0 : int.MaxValue - 100000; // Avoid overflow in addition
            }
        }

        // Top-left to bottom-right
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (matrix[i][j] != 0) {
                    if (i > 0) {
                        distances[i][j] = Math.Min(distances[i][j], distances[i-1][j] + 1);
                    }
                    if (j > 0) {
                        distances[i][j] = Math.Min(distances[i][j], distances[i][j-1] + 1);
                    }
                }
            }
        }

        // Bottom-right to top-left
        for (int i = rows - 1; i >= 0; i--) {
            for (int j = cols - 1; j >= 0; j--) {
                if (i < rows - 1) {
                    distances[i][j] = Math.Min(distances[i][j], distances[i+1][j] + 1);
                }
                if (j < cols - 1) {
                    distances[i][j] = Math.Min(distances[i][j], distances[i][j+1] + 1);
                }
            }
        }
        return distances;
    }
}
```

- **Time Complexity:** O(rows * cols), two complete passes over the data.
- **Space Complexity:** O(1), as we modify the input matrix directly.

### Approach 3: Optimized BFS with a 2-pass Solution

This uses the first two methods but optimizes BFS by reducing unnecessary checks using a two-pass solution similar to dynamic programming. This method can save some overhead in BFS initialization. Although very similar to the dynamic programming approach, careful attention is paid so initial BFS fills in skews of the grid minimizing the work in the main passes. The same code and logic apply for this optimization.

- **Time Complexity:** O(rows * cols)
- **Space Complexity:** O(rows * cols) for BFS queue usage, reducible to O(1) when done during passes.

