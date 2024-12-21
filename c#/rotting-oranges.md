## [Leetcode: Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

### Approaches
1. [Breadth-First Search (BFS) Approach](#bfs-approach)
2. [Optimized BFS with Early Exit](#optimized-bfs-with-early-exit)

### BFS Approach
#### Intuition
The problem can be visualized as a multi-source BFS problem where each rotten orange spreads the rottenness to its neighboring fresh oranges. The goal is to determine the minimum time taken for all fresh oranges to rot, or return -1 if not all oranges can become rotten. 

We start by enqueuing all initial rotten oranges and then perform BFS to spread their rot to adjacent oranges, keeping track of time in a step-wise manner.

#### Steps:
1. First, count all fresh oranges and enqueue all rotten oranges with their coordinates and the initial time stamp of 0.
2. Use a queue to process each orange: dequeue an orange, and spread its rot to any unvisited adjacent fresh orange, enqueuing the newly rotten ones.
3. For each orange rotted, decrease the count of fresh oranges.
4. Continue the process until the queue is empty.
5. If there are no fresh oranges left, return the time it took. Otherwise, return -1 if any are left.

```csharp
public class Solution {
    public int OrangesRotting(int[][] grid) {
        if (grid == null || grid.Length == 0) return -1;

        int rows = grid.Length;
        int cols = grid[0].Length;
        int freshCount = 0;
        Queue<(int, int, int)> queue = new Queue<(int, int, int)>();

        // Enqueue initial rotten oranges and count fresh oranges
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 2) {
                    queue.Enqueue((r, c, 0)); // Add with initial time 0
                }
                else if (grid[r][c] == 1) {
                    freshCount++;
                }
            }
        }

        // Directions for the 4 neighboring cells (up, down, left, right)
        int[][] directions = new int[][] {
            new int[] {1, 0}, new int[] {-1, 0},
            new int[] {0, 1}, new int[] {0, -1}
        };

        int time = 0;

        // Perform BFS
        while (queue.Count > 0) {
            var (currentRow, currentCol, currentTime) = queue.Dequeue();
            foreach (var dir in directions) {
                int newRow = currentRow + dir[0];
                int newCol = currentCol + dir[1];

                // If any of the neighbors are a fresh orange, rot it
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1) {
                    grid[newRow][newCol] = 2; // Rot the orange
                    queue.Enqueue((newRow, newCol, currentTime + 1)); // Enqueue new rotten orange
                    freshCount--; // Decrease fresh orange count
                    time = currentTime + 1; // Update time
                }
            }
        }

        return freshCount == 0 ? time : -1;
    }
}
```
**Time Complexity:** O(N), where N is the number of cells in the grid, since each cell is processed at most once.

**Space Complexity:** O(N), for the queue data structure utilized in storing cell coordinates.

### Optimized BFS with Early Exit
#### Intuition
The previously described BFS approach efficiently finds the time needed to rot all oranges but it can be slightly optimized to exit early when no more fresh oranges remain, potentially saving some unnecessary cycles through the queue.

#### Steps:
- Implement the same BFS logic as above.
- Add an early exit mechanism in the loop: If the `freshCount` drops to zero, exit the loop immediately and return the time.

```csharp
public class Solution {
    public int OrangesRotting(int[][] grid) {
        if (grid == null || grid.Length == 0) return -1;

        int rows = grid.Length;
        int cols = grid[0].Length;
        int freshCount = 0;
        Queue<(int, int, int)> queue = new Queue<(int, int, int)>();

        // Enqueue initial rotten oranges and count fresh oranges
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] == 2) {
                    queue.Enqueue((r, c, 0)); // Add with initial time 0
                }
                else if (grid[r][c] == 1) {
                    freshCount++;
                }
            }
        }

        int[][] directions = new int[][] {
            new int[] {1, 0}, new int[] {-1, 0},
            new int[] {0, 1}, new int[] {0, -1}
        };

        int time = 0;

        while (queue.Count > 0) {
            var (currentRow, currentCol, currentTime) = queue.Dequeue();
            foreach (var dir in directions) {
                int newRow = currentRow + dir[0];
                int newCol = currentCol + dir[1];
                if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1) {
                    grid[newRow][newCol] = 2;
                    queue.Enqueue((newRow, newCol, currentTime + 1));
                    freshCount--;
                    time = currentTime + 1;

                    // Early exit: If no fresh oranges left, return time immediately
                    if (freshCount == 0) return time;
                }
            }
        }

        return freshCount == 0 ? time : -1;
    }
}
```
**Time Complexity:** O(N), similar to the plain BFS.

**Space Complexity:** O(N), due to the queue.

