### [LeetCode 64: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

#### Approaches:
- [Approach 1: Recursive (Top-Down) Approach](#approach-1-recursive-top-down-approach)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)
- [Approach 3: Dynamic Programming with Iteration (Bottom-Up)](#approach-3-dynamic-programming-with-iteration-bottom-up)

---

#### Approach 1: Recursive (Top-Down) Approach

**Intuition:**

This approach involves choosing between moving right or downward from the current cell. The main idea is to recursively explore all possible paths from the top-left to the bottom-right of the grid and find the path with the minimum sum. 

Key points:
- At any given cell `(i, j)`, the minimum path sum to this cell would be the value in the current cell plus the minimum path sum from either the cell directly above `(i-1, j)` or the cell to the left `(i, j-1)`.
- This is a straightforward recursive approach, but it results in overlapping subproblems, which can be optimized using dynamic programming.

**Time Complexity:** Exponential, O(2^(m+n)), where `m` is the number of rows and `n` is the number of columns. 

**Space Complexity:** O(m + n), due to the call stack.

```typescript
function minPathSum(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;

    function recurse(i: number, j: number): number {
        // Base case: when we reach the top left corner
        if (i === 0 && j === 0) {
            return grid[i][j];
        }
        
        // Out of bounds check
        if (i < 0 || j < 0) {
            return Infinity;
        }

        // Recursive case: current cell value plus the minimum path sum from top or left
        return grid[i][j] + Math.min(recurse(i - 1, j), recurse(i, j - 1));
    }

    return recurse(m - 1, n - 1);
}
```

---

#### Approach 2: Dynamic Programming with Memoization

**Intuition:**

The recursive solution has overlapping sub-problems which can be cached using a memoization technique. This will save previously computed results and reduce the number of computations.

**Time Complexity:** O(m*n), where `m` is the number of rows and `n` is the number of columns in the grid.

**Space Complexity:** O(m*n) due to the additional space used for the memoization table.

```typescript
function minPathSum(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;
    const memo: number[][] = Array.from({ length: m }, () => Array(n).fill(-1));

    function recurse(i: number, j: number): number {
        if (i < 0 || j < 0) {
            return Infinity;
        }
        if (i === 0 && j === 0) {
            return grid[i][j];
        }
        if (memo[i][j] !== -1) {
            return memo[i][j];
        }
        memo[i][j] = grid[i][j] + Math.min(recurse(i - 1, j), recurse(i, j - 1));
        return memo[i][j];
    }

    return recurse(m - 1, n - 1);
}
```

---

#### Approach 3: Dynamic Programming with Iteration (Bottom-Up)

**Intuition:**

Utilize a bottom-up strategy with an iterative approach to fill up a `dp` table where `dp[i][j]` represents the minimum path sum to reach cell `(i, j)`. The value is obtained by adding the current cell's value to the minimum of the top cell and left cell values.

Steps:
- Initialize the `dp` table with the same dimensions as the input grid.
- Start from the top-left corner and iterate through the grid, updating the table.
- The grid itself can be used to store the `dp` values, reducing space complexity.

**Time Complexity:** O(m*n)

**Space Complexity:** O(1) when modifying the input grid directly.

```typescript
function minPathSum(grid: number[][]): number {
    const m = grid.length;
    const n = grid[0].length;

    // Initialize first row (cumulative sum from left)
    for (let j = 1; j < n; j++) {
        grid[0][j] += grid[0][j - 1];
    }

    // Initialize first column (cumulative sum from top)
    for (let i = 1; i < m; i++) {
        grid[i][0] += grid[i - 1][0];
    }

    // Fill the rest of the grid
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
        }
    }

    return grid[m - 1][n - 1];
}
```
Each solution shows increasingly efficient methods for computing the minimum path sum, culminating in the most efficient iterative dp solution that modifies the grid in place.

