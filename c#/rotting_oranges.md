# 994. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)

## Approach: Breadth-First Search (BFS)

### Solution
csharp
```csharp
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the queue and visited cells
using System.Collections.Generic;

public class Solution {
    public int OrangesRotting(int[][] grid) {
        int rows = grid.Length;
        int cols = grid[0].Length;
        Queue<int[]> queue = new Queue<int[]>();
        int freshCount = 0;

        // Step 1: Initialize the queue with all rotten oranges and count fresh oranges
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 2) {
                    queue.Enqueue(new int[] { i, j });
                } else if (grid[i][j] == 1) {
                    freshCount++;
                }
            }
        }

        // Step 2: Perform BFS to rot adjacent fresh oranges
        int minutes = 0;
        int[][] directions = { new int[] { 0, 1 }, new int[] { 1, 0 }, new int[] { 0, -1 }, new int[] { -1, 0 } };

        while (queue.Count > 0 && freshCount > 0) {
            int size = queue.Count;
            for (int i = 0; i < size; i++) {
                int[] current = queue.Dequeue();
                foreach (int[] dir in directions) {
                    int newRow = current[0] + dir[0];
                    int newCol = current[1] + dir[1];

                    // Check bounds and if the orange is fresh
                    if (newRow >= 0 && newRow < rows && newCol >= 0 && newCol < cols && grid[newRow][newCol] == 1) {
                        grid[newRow][newCol] = 2; // Rot the orange
                        queue.Enqueue(new int[] { newRow, newCol });
                        freshCount--;
                    }
                }
            }
            minutes++;
        }

        // If there are still fresh oranges left, return -1
        return freshCount == 0 ? minutes : -1;
    }
}
```

