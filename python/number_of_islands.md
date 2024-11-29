# 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach: Depth-First Search (DFS)

### Solution
python
```python
# Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
# Space Complexity: O(m * n), for the recursion stack in the worst case
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        if not grid:
            return 0

        rows = len(grid)
        cols = len(grid[0])
        num_islands = 0

        def dfs(grid, row, col):
            # Base case: Check bounds and water
            if row < 0 or row >= rows or col < 0 or col >= cols or grid[row][col] == '0':
                return

            grid[row][col] = '0'  # Mark the current cell as visited

            # Explore all 4 directions
            dfs(grid, row - 1, col)  # Up
            dfs(grid, row + 1, col)  # Down
            dfs(grid, row, col - 1)  # Left
            dfs(grid, row, col + 1)  # Right
        
        # Iterate over every cell in the grid
        for i in range(rows):
            for j in range(cols):
                if grid[i][j] == '1':
                    num_islands += 1  # Found a new island
                    dfs(grid, i, j)  # Mark all connected land as visited
        
        return num_islands
```

