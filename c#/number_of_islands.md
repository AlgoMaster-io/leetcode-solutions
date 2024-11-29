# 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach: Depth-First Search (DFS)

### Solution

csharp
```csharp
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the recursion stack in the worst case
public class Solution {
    public int NumIslands(char[][] grid) {
        int rows = grid.Length;
        int cols = grid[0].Length;
        int numIslands = 0;

        // Iterate over every cell in the grid
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    numIslands++; // Found a new island
                    DFS(grid, i, j); // Mark all connected land as visited
                }
            }
        }

        return numIslands;
    }

    private void DFS(char[][] grid, int row, int col) {
        // Base case: Check bounds and water
        if (row < 0 || row >= grid.Length || col < 0 || col >= grid[0].Length || grid[row][col] == '0') {
            return;
        }

        grid[row][col] = '0'; // Mark the current cell as visited

        // Explore all 4 directions
        DFS(grid, row - 1, col); // Up
        DFS(grid, row + 1, col); // Down
        DFS(grid, row, col - 1); // Left
        DFS(grid, row, col + 1); // Right
    }
}
```

