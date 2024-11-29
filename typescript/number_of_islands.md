# 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach: Depth-First Search (DFS)

### Solution
typescript
```typescript
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the recursion stack in the worst case
class Solution {
    numIslands(grid: string[][]): number {
        const rows = grid.length;
        const cols = grid[0].length;
        let numIslands = 0;

        // Iterate over every cell in the grid
        for (let i = 0; i < rows; i++) {
            for (let j = 0; j < cols; j++) {
                if (grid[i][j] === '1') {
                    numIslands++; // Found a new island
                    this.dfs(grid, i, j); // Mark all connected land as visited
                }
            }
        }

        return numIslands;
    }

    private dfs(grid: string[][], row: number, col: number): void {
        // Base case: Check bounds and water
        if (row < 0 || row >= grid.length || col < 0 || col >= grid[0].length || grid[row][col] === '0') {
            return;
        }

        grid[row][col] = '0'; // Mark the current cell as visited

        // Explore all 4 directions
        this.dfs(grid, row - 1, col); // Up
        this.dfs(grid, row + 1, col); // Down
        this.dfs(grid, row, col - 1); // Left
        this.dfs(grid, row, col + 1); // Right
    }
}
```

