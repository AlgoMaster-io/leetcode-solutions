# 63. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Approach 1: Dynamic Programming with a 2D Array

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
function uniquePathsWithObstacles(obstacleGrid) {
    const m = obstacleGrid.length;
    const n = obstacleGrid[0].length;

    // If the start or end is an obstacle, no paths are possible
    if (obstacleGrid[0][0] === 1 || obstacleGrid[m - 1][n - 1] === 1) {
        return 0;
    }

    const dp = Array.from({ length: m }, () => Array(n).fill(0));
    dp[0][0] = 1;  // Start position

    // Initialize first column
    for (let i = 1; i < m; i++) {
        dp[i][0] = (obstacleGrid[i][0] === 0 && dp[i - 1][0] === 1) ? 1 : 0;
    }

    // Initialize first row
    for (let j = 1; j < n; j++) {
        dp[0][j] = (obstacleGrid[0][j] === 0 && dp[0][j - 1] === 1) ? 1 : 0;
    }

    // Fill the DP array
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (obstacleGrid[i][j] === 0) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            } else {
                dp[i][j] = 0;  // An obstacle
            }
        }
    }

    return dp[m - 1][n - 1];  // The bottom-right corner
}
```

## Approach 2: Dynamic Programming with a 1D Array

### Solution
```javascript
// Time Complexity: O(m * n)
// Space Complexity: O(n)
function uniquePathsWithObstacles(obstacleGrid) {
    const m = obstacleGrid.length;
    const n = obstacleGrid[0].length;
    
    // Array to store current row's path counts
    const dp = Array(n).fill(0);

    dp[0] = (obstacleGrid[0][0] === 0) ? 1 : 0;  // Start position

    // Fill for the first row
    for (let j = 1; j < n; j++) {
        dp[j] = (obstacleGrid[0][j] === 0) ? dp[j - 1] : 0;
    }

    // Process the rest of the rows
    for (let i = 1; i < m; i++) {
        // Update first column of the current row
        dp[0] = (obstacleGrid[i][0] === 0 && dp[0] === 1) ? 1 : 0;
        for (let j = 1; j < n; j++) {
            if (obstacleGrid[i][j] === 0) {
                dp[j] += dp[j - 1]; // Current cell is a free path
            } else {
                dp[j] = 0;  // Obstacle found, no paths through this cell
            }
        }
    }

    return dp[n - 1];  // The bottom-right corner
}
```

