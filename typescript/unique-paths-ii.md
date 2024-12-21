# [Leetcode 63: Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Navigation
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

## Approach 1: Recursive Solution

### Intuition
A straightforward approach to tackling this problem is to use recursion to explore all possible paths from the start (top-left corner) to the destination (bottom-right corner). At any given cell, we have two choices: move right or move down, provided these moves do not land on an obstacle. If we encounter an obstacle, the path is blocked, and we have zero paths from that point.

### Code
```typescript
function uniquePathsWithObstacles(obstacleGrid: number[][]): number {
    const rows = obstacleGrid.length;
    const cols = obstacleGrid[0].length;

    function helper(r: number, c: number): number {
        // If out of bounds or on an obstacle, return 0 paths
        if (r < 0 || c < 0 || obstacleGrid[r][c] === 1) return 0;
        // If at the start, there's only one path (start itself)
        if (r === 0 && c === 0) return 1;

        // Explore paths from the top and the left
        const pathsFromTop = helper(r - 1, c);
        const pathsFromLeft = helper(r, c - 1);

        return pathsFromTop + pathsFromLeft;
    }

    return helper(rows - 1, cols - 1);
}
```

### Complexity
- **Time Complexity**: Exponential, \(O(2^{(m \times n)})\) in the worst case due to overlapping subproblems and repeated calculations.
- **Space Complexity**: \(O(m \times n)\) due to the recursive call stack depth.

---

## Approach 2: Dynamic Programming

### Intuition
The recursive solution has a lot of overlapping subproblems. A dynamic programming approach can solve this by using a 2D array to store the number of paths to each cell, filling up the grid progressively.

1. Initialize a `dp` table where `dp[i][j]` holds the number of unique paths to reach the cell `(i, j)`.
2. If an obstacle is found at the start, immediately return `0`, as no paths are possible.
3. Initialize the first row and first column, as they can only be reached from one direction.
4. Fill up the `dp` grid by summing the paths from the cell above and the cell to the left, provided there is no obstacle.

### Code
```typescript
function uniquePathsWithObstacles(obstacleGrid: number[][]): number {
    const rows = obstacleGrid.length;
    const cols = obstacleGrid[0].length;

    // If the start or the end is blocked
    if (obstacleGrid[0][0] === 1 || obstacleGrid[rows - 1][cols - 1] === 1) return 0;

    // Initialize the dp array
    const dp: number[][] = Array.from({ length: rows }, () => Array(cols).fill(0));

    // Initializing the first cell
    dp[0][0] = 1;

    // Fill the first row
    for (let i = 1; i < cols; i++) {
        dp[0][i] = obstacleGrid[0][i] === 0 ? dp[0][i - 1] : 0;
    }

    // Fill the first column
    for (let j = 1; j < rows; j++) {
        dp[j][0] = obstacleGrid[j][0] === 0 ? dp[j - 1][0] : 0;
    }

    // Fill the rest of dp table
    for (let r = 1; r < rows; r++) {
        for (let c = 1; c < cols; c++) {
            if (obstacleGrid[r][c] === 0) {
                dp[r][c] = dp[r - 1][c] + dp[r][c - 1];
            } else {
                dp[r][c] = 0;
            }
        }
    }

    return dp[rows - 1][cols - 1];
}
```

### Complexity
- **Time Complexity**: \(O(m \times n)\), where \(m\) is the number of rows and \(n\) is the number of columns, as we fill each cell exactly once.
- **Space Complexity**: \(O(m \times n)\) due to the dp table storing results for each cell.

