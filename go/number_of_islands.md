# 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach: Depth-First Search (DFS)

### Solution
```go
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the recursion stack in the worst case
package main

func numIslands(grid [][]byte) int {
    rows := len(grid)
    cols := len(grid[0])
    numIslands := 0

    // Iterate over every cell in the grid
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if grid[i][j] == '1' {
                numIslands++ // Found a new island
                dfs(grid, i, j) // Mark all connected land as visited
            }
        }
    }

    return numIslands
}

func dfs(grid [][]byte, row, col int) {
    // Base case: Check bounds and water
    if row < 0 || row >= len(grid) || col < 0 || col >= len(grid[0]) || grid[row][col] == '0' {
        return
    }

    grid[row][col] = '0' // Mark the current cell as visited

    // Explore all 4 directions
    dfs(grid, row - 1, col) // Up
    dfs(grid, row + 1, col) // Down
    dfs(grid, row, col - 1) // Left
    dfs(grid, row, col + 1) // Right
}
```

