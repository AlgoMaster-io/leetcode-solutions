# [Unique Paths II - Leetcode Problem 63](https://leetcode.com/problems/unique-paths-ii/)

## Approaches

1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)
3. [Dynamic Programming - Optimal Space](#dynamic-programming---optimal-space)

---

## Recursive Approach

### Intuition
In this approach, we recursively explore each possible path starting from the top-left corner to the bottom-right corner of the grid. At each cell, we have the option to move either down or right, provided that the next cell is within bounds and not blocked by an obstacle. This approach will explore all potential paths and count how many of them reach the destination.

### Code

```javascript
function uniquePathsWithObstacles(grid) {
    const rows = grid.length;
    const cols = grid[0].length;

    function dfs(r, c) {
        // If out of bounds or on an obstacle, disallow this path
        if (r >= rows || c >= cols || grid[r][c] === 1) return 0;
        
        // Reached the destination
        if (r === rows - 1 && c === cols - 1) return 1;
        
        // Explore the path moving right and down recursively
        return dfs(r + 1, c) + dfs(r, c + 1);
    }

    return dfs(0, 0);
}
```

### Complexity
- Time Complexity: `O(2^(m+n))` where `m` is the number of rows and `n` is the number of columns, due to possible overlapping subproblems and recomputation.
- Space Complexity: `O(m + n)` due to recursion stack.


## Dynamic Programming with Memoization

### Intuition
The recursive approach has many overlapping subproblems, which can be optimized using memoization. We use a memoization table to store already computed results of subproblems, thus avoiding redundant computations.

### Code

```javascript
function uniquePathsWithObstacles(grid) {
    const rows = grid.length;
    const cols = grid[0].length;
    const memo = Array.from({ length: rows }, () => Array(cols).fill(-1));

    function dfs(r, c) {
        if (r >= rows || c >= cols || grid[r][c] === 1) return 0;
        if (r === rows - 1 && c === cols - 1) return 1;
        
        // If already computed, return the stored result
        if (memo[r][c] !== -1) return memo[r][c];
        
        // Compute the result and store it in memo
        memo[r][c] = dfs(r + 1, c) + dfs(r, c + 1);
        return memo[r][c];
    }

    return dfs(0, 0);
}
```

### Complexity
- Time Complexity: `O(m * n)` since each cell is computed once.
- Space Complexity: `O(m * n)` due to the memoization table.

## Dynamic Programming - Optimal Space

### Intuition
We can further optimize the space complexity by using a single-dimensional array. The idea is to update the number of ways to reach each cell by considering only the current and previous rows. This transformation allows us to compute the result iteratively without needing the full grid.

### Code

```javascript
function uniquePathsWithObstacles(grid) {
    const rows = grid.length;
    const cols = grid[0].length;

    // If the starting point itself is blocked, no paths can proceed
    if (grid[0][0] === 1) return 0;

    // The dp array represents the number of paths reaching each cell in the current row
    const dp = Array(cols).fill(0);
    dp[0] = 1; // Starting position

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (grid[r][c] === 1) {
                dp[c] = 0; // If the cell is an obstacle, zero paths can lead to it
            } else if (c > 0) {
                dp[c] += dp[c - 1]; // Current cell paths + previous cell paths in row
            }
        }
    }

    // The last element in dp represents the number of unique paths to the bottom-right corner
    return dp[cols - 1];
}
```

### Complexity
- Time Complexity: `O(m * n)` due to iterating over all cells.
- Space Complexity: `O(n)` due to single-dimensional storage.

