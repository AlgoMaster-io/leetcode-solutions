# 63. [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Approach 1: Dynamic Programming with a 2D Array

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(m * n)
package main

func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    m := len(obstacleGrid)
    n := len(obstacleGrid[0])
    
    // If the start or end is an obstacle, no paths are possible
    if obstacleGrid[0][0] == 1 || obstacleGrid[m-1][n-1] == 1 {
        return 0
    }

    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    dp[0][0] = 1 // Start position

    // Initialize first column
    for i := 1; i < m; i++ {
        if obstacleGrid[i][0] == 0 && dp[i-1][0] == 1 {
            dp[i][0] = 1
        } else {
            dp[i][0] = 0
        }
    }

    // Initialize first row
    for j := 1; j < n; j++ {
        if obstacleGrid[0][j] == 0 && dp[0][j-1] == 1 {
            dp[0][j] = 1
        } else {
            dp[0][j] = 0
        }
    }

    // Fill the DP array
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            if obstacleGrid[i][j] == 0 {
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
            } else {
                dp[i][j] = 0 // An obstacle
            }
        }
    }

    return dp[m-1][n-1] // The bottom-right corner
}
```

## Approach 2: Dynamic Programming with a 1D Array

### Solution
go
```go
// Time Complexity: O(m * n)
// Space Complexity: O(n)
package main

func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    m := len(obstacleGrid)
    n := len(obstacleGrid[0])
    
    // Array to store current row's path counts
    dp := make([]int, n)

    dp[0] = 0
    if obstacleGrid[0][0] == 0 {
        dp[0] = 1 // Start position
    }

    // Fill for the first row
    for j := 1; j < n; j++ {
        if obstacleGrid[0][j] == 0 {
            dp[j] = dp[j-1]
        } else {
            dp[j] = 0
        }
    }

    // Process the rest of the rows
    for i := 1; i < m; i++ {
        // Update first column of the current row
        if obstacleGrid[i][0] == 0 && dp[0] == 1 {
            dp[0] = 1
        } else {
            dp[0] = 0
        }
        for j := 1; j < n; j++ {
            if obstacleGrid[i][j] == 0 {
                dp[j] += dp[j-1] // Current cell is a free path
            } else {
                dp[j] = 0 // Obstacle found, no paths through this cell
            }
        }
    }

    return dp[n-1] // The bottom-right corner
}
```

