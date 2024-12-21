# [Leetcode 329: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

## Approaches

1. [DFS (Depth First Search)](#dfs-depth-first-search)
2. [DFS with Memoization](#dfs-with-memoization)

## DFS (Depth First Search)

### Intuition
The problem requires us to find the longest increasing path in a matrix, which can be solved by exploring all paths using Depth First Search (DFS). The idea is to perform a DFS for each cell and track the maximum length of increasing paths. However, this basic approach can be very inefficient, as it explores each path multiple times.

### Implementation
Here is a straightforward implementation using DFS without optimization:

```typescript
function longestIncreasingPath(matrix: number[][]): number {
    const rows = matrix.length;
    const cols = matrix[0].length;
    
    // Helper function to perform DFS
    function dfs(row: number, col: number, prevVal: number): number {
        // If out of bounds or not an increasing step, return 0
        if (row < 0 || row >= rows || col < 0 || col >= cols || matrix[row][col] <= prevVal) {
            return 0;
        }
        
        // Explore all possible 4 directions (up, down, left, right)
        const value = matrix[row][col];
        return 1 + Math.max(
            dfs(row + 1, col, value),
            dfs(row - 1, col, value),
            dfs(row, col + 1, value),
            dfs(row, col - 1, value)
        );
    }
    
    let maxPath = 0;

    // Try to find the longest path starting from each cell
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            maxPath = Math.max(maxPath, dfs(r, c, Number.MIN_SAFE_INTEGER));
        }
    }

    return maxPath;
}
```

### Time Complexity
- O(2^n): In the worst case, for each cell, we could explore all paths multiple times.

### Space Complexity
- O(n): Stack space used by recursion.

---

## DFS with Memoization

### Intuition
To improve the efficiency of the basic DFS approach, we use memoization. The idea is to store the result of the maximum increasing path starting from each cell, so we do not recompute it when encountered again.

### Implementation
Here is the optimized implementation using DFS with memoization:

```typescript
function longestIncreasingPath(matrix: number[][]): number {
    const rows = matrix.length;
    const cols = matrix[0].length;
    const cache: number[][] = Array.from({ length: rows }, () => Array(cols).fill(0));

    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];

    // Helper function to perform DFS with memoization
    function dfs(row: number, col: number): number {
        // If result is already computed
        if (cache[row][col] !== 0) {
            return cache[row][col];
        }

        let maxLength = 1;

        // Explore all possible 4 directions (up, down, left, right)
        for (const [dr, dc] of directions) {
            const newRow = row + dr;
            const newCol = col + dc;

            if (newRow < 0 || newRow >= rows || newCol < 0 || newCol >= cols || matrix[newRow][newCol] <= matrix[row][col]) {
                continue;
            }

            const length = 1 + dfs(newRow, newCol);
            maxLength = Math.max(maxLength, length);
        }

        cache[row][col] = maxLength;
        return maxLength;
    }

    let maxPath = 0;

    // Try to find the longest path starting from each cell
    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            maxPath = Math.max(maxPath, dfs(r, c));
        }
    }

    return maxPath;
}
```

### Time Complexity
- O(m * n): Each cell is computed only once and results are cached.

### Space Complexity
- O(m * n): Used for caching results of each cell.

The DFS with memoization provides an efficient way to solve the problem, ensuring that each path is only computed once. This greatly reduces the time complexity compared to the basic DFS approach.

