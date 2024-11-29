# 200. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

## Approach: Depth-First Search (DFS)

### Solution
c++
// Time Complexity: O(m * n), where m is the number of rows and n is the number of columns
// Space Complexity: O(m * n), for the recursion stack in the worst case
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int rows = grid.size();
        int cols = grid[0].size();
        int numIslands = 0;

        // Iterate over every cell in the grid
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    numIslands++; // Found a new island
                    dfs(grid, i, j); // Mark all connected land as visited
                }
            }
        }

        return numIslands;
    }

private:
    void dfs(vector<vector<char>>& grid, int row, int col) {
        // Base case: Check bounds and water
        if (row < 0 || row >= grid.size() || col < 0 || col >= grid[0].size() || grid[row][col] == '0') {
            return;
        }

        grid[row][col] = '0'; // Mark the current cell as visited

        // Explore all 4 directions
        dfs(grid, row - 1, col); // Up
        dfs(grid, row + 1, col); // Down
        dfs(grid, row, col - 1); // Left
        dfs(grid, row, col + 1); // Right
    }
};
```

