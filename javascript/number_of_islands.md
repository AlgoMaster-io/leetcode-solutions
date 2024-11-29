# 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach: Depth-First Search (DFS)

### Solution
```javascript
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the recursion stack in the worst case
var numIslands = function(grid) {
    let rows = grid.length;
    let cols = grid[0].length;
    let numIslands = 0;

    // Iterate over every cell in the grid
    for (let i = 0; i < rows; i++) {
        for (let j = 0; j < cols; j++) {
            if (grid[i][j] === '1') {
                numIslands++; // Found a new island
                dfs(grid, i, j); // Mark all connected land as visited
            }
        }
    }

    return numIslands;
};

var dfs = function(grid, row, col) {
    // Base case: Check bounds and water
    if (row < 0 || row >= grid.length || col < 0 || col >= grid[0].length || grid[row][col] === '0') {
        return;
    }

    grid[row][col] = '0'; // Mark the current cell as visited

    // Explore all 4 directions
    dfs(grid, row - 1, col); // Up
    dfs(grid, row + 1, col); // Down
    dfs(grid, row, col - 1); // Left
    dfs(grid, row, col + 1); // Right
};
```

