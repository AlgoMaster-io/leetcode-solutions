# [Leetcode 64: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Solutions:
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Dynamic Programming - Bottom Up](#approach-2-dynamic-programming-bottom-up)
- [Approach 3: Dynamic Programming - Space Optimized](#approach-3-dynamic-programming-space-optimized)

---

### Approach 1: Recursive Approach

**Intuition:**

The problem can be visualized as finding paths in a grid, and we can use recursion to explore potential paths. The idea is to start from the top-left corner `(0,0)` and explore paths to reach the bottom-right corner `(m-1, n-1)`. At each step in a cell `(i, j)`, we can either move right `(i, j+1)` or down `(i+1, j)`. The goal is to minimize the sum of numbers along the path from the top-left to the bottom-right of the grid.

**Code:**

```javascript
function minPathSum(grid) {
    function findMinPath(i, j) {
        // Base Case: If out of boundaries, return Infinity since it's an invalid path
        if (i >= grid.length || j >= grid[0].length) return Infinity;
        // Base Case: If reached at the bottom-right cell, return its value
        if (i === grid.length - 1 && j === grid[0].length - 1) return grid[i][j];
        
        // Recursive calculation for right and down path
        const rightPath = findMinPath(i, j + 1);
        const downPath = findMinPath(i + 1, j);
        
        // Current cell value + minimum cost of the two possible paths
        return grid[i][j] + Math.min(rightPath, downPath);
    }
    return findMinPath(0, 0);
}
```

**Time Complexity:** \(O(2^{(m+n)})\) because we could explore all possible paths.

**Space Complexity:** \(O(m+n)\) for the recursion stack space.

---

### Approach 2: Dynamic Programming - Bottom Up

**Intuition:**

An improvement over the naive recursion is to use dynamic programming, where we construct a table (`dp`) to store the minimum path sums up to each cell. The logic is to calculate the minimum path sums starting from `(0, 0)` to `(m-1, n-1)`.

**Code:**

```javascript
function minPathSum(grid) {
    let m = grid.length;
    let n = grid[0].length;
    
    // Create a DP table to store the minimum path sums
    const dp = Array.from({ length: m }, () => Array(n).fill(0));
    dp[0][0] = grid[0][0];
    
    // Fill the first row (only move right)
    for (let j = 1; j < n; j++) {
        dp[0][j] = dp[0][j - 1] + grid[0][j];
    }
    
    // Fill the first column (only move down)
    for (let i = 1; i < m; i++) {
        dp[i][0] = dp[i - 1][0] + grid[i][0];
    }
    
    // Fill the rest of the dp array
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = grid[i][j] + Math.min(dp[i - 1][j], dp[i][j - 1]);
        }
    }
    
    return dp[m - 1][n - 1];
}
```

**Time Complexity:** \(O(m \times n)\) because we compute the dp values for each cell once.

**Space Complexity:** \(O(m \times n)\) because of the additional dp array.

---

### Approach 3: Dynamic Programming - Space Optimized

**Intuition:**

We can optimize space since the computation for each cell only needs the current row/column and the previous one. Hence, we can use a 1D array to accomplish this by continuously updating it for each row.

**Code:**

```javascript
function minPathSum(grid) {
    let m = grid.length;
    let n = grid[0].length;
    
    // Create a 1D array to store the current state
    const dp = Array(n).fill(0);
    dp[0] = grid[0][0];
    
    // Initialize the first row
    for (let j = 1; j < n; j++) {
        dp[j] = dp[j - 1] + grid[0][j];
    }
    
    // Compute for the rest of the grid row by row
    for (let i = 1; i < m; i++) {
        dp[0] += grid[i][0]; // First column update is only from top
        for (let j = 1; j < n; j++) {
            dp[j] = grid[i][j] + Math.min(dp[j], dp[j - 1]);
        }
    }
    
    return dp[n - 1];
}
```

**Time Complexity:** \(O(m \times n)\) for computing the value for each cell.

**Space Complexity:** \(O(n)\) for the 1D array storing one row/column of computations.

