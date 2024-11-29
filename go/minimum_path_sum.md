# 64. [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approach 1: Recursion (Brute force)

### Solution
go
```go
// Time Complexity: O(2^(m+n))
// Space Complexity: O(m+n) - due to the recursion stack
package main
import "math"

func minPathSum(grid [][]int) int {
    return calculate(grid, len(grid)-1, len(grid[0])-1)
}

// Recursive function to find the minimum path sum to reach cell (i, j)
func calculate(grid [][]int, i, j int) int {
    // Base cases for boundaries
    if i == 0 && j == 0 {
        return grid[0][0] // Start point
    }
    if i < 0 || j < 0 {
        return math.MaxInt32 // Out of grid
    }

    // Calculate minimum path sum to current cell from either top or left
    return grid[i][j] + min(calculate(grid, i-1, j), calculate(grid, i, j-1))
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## Approach 2: Recursion with Memoization

### Solution
go
```go
// Time Complexity: O(m*n)
// Space Complexity: O(m*n) - for the memoization table
package main
import "math"

func minPathSum(grid [][]int) int {
    memo := make([][]int, len(grid))
    for i := range memo {
        memo[i] = make([]int, len(grid[0]))
    }
    return calculate(grid, len(grid)-1, len(grid[0])-1, memo)
}

// Recursive function with memoization to find the minimum path sum to reach cell (i, j)
func calculate(grid [][]int, i, j int, memo [][]int) int {
    // Base cases for boundaries
    if i == 0 && j == 0 {
        return grid[0][0] // Start point
    }
    if i < 0 || j < 0 {
        return math.MaxInt32 // Out of grid
    }

    if memo[i][j] != 0 {
        return memo[i][j] // Return already computed result
    }

    // Calculate minimum path sum to current cell from either top or left
    memo[i][j] = grid[i][j] + min(calculate(grid, i-1, j, memo), calculate(grid, i, j-1, memo))
    return memo[i][j]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## Approach 3: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(m*n)
// Space Complexity: O(m*n)
package main

func minPathSum(grid [][]int) int {
    m := len(grid)
    n := len(grid[0])
    
    // Create a DP table to store the minimum path sum up to each cell
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    dp[0][0] = grid[0][0] // Initialize the starting point
    
    // Fill the first row (can only come from the left)
    for i := 1; i < n; i++ {
        dp[0][i] = dp[0][i-1] + grid[0][i]
    }

    // Fill the first column (can only come from above)
    for i := 1; i < m; i++ {
        dp[i][0] = dp[i-1][0] + grid[i][0]
    }

    // Fill the rest of the dp table
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            // Minimum of coming from the top or the left
            dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
        }
    }

    return dp[m-1][n-1] // Return the last cell which contains the result
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## Approach 4: Dynamic Programming with Space Optimization

### Solution
go
```go
// Time Complexity: O(m*n)
// Space Complexity: O(n)
package main

func minPathSum(grid [][]int) int {
    m := len(grid)
    n := len(grid[0])
    
    // Use a 1D dp array to save space
    dp := make([]int, n)
    dp[0] = grid[0][0] // Initialize the starting point
    
    // Fill the first row (can only come from the left)
    for i := 1; i < n; i++ {
        dp[i] = dp[i-1] + grid[0][i]
    }

    // Fill for each row using the dp array
    for i := 1; i < m; i++ {
        dp[0] += grid[i][0] // Update the first column
        for j := 1; j < n; j++ {
            dp[j] = grid[i][j] + min(dp[j], dp[j-1]) // Update by choosing minimum of top or left
        }
    }

    return dp[n-1] // Return the last cell which contains the result
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

