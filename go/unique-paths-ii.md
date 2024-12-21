
[LeetCode Problem 63: Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Recursive Approach

The recursive approach is a simple backtracking solution where we explore every possible path. Starting from the top-left corner, at each step, we recursively try moving in the down and right directions if those cells are valid and not blocked.

#### Intuition
- At each point, decide to move either right or down.
- Stop recursion if we move out of bounds or encounter an obstacle.
- If we reach the destination, a valid path is found.

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    return findPaths(0, 0, obstacleGrid)
}

func findPaths(i, j int, obstacleGrid [][]int) int {
    // Check if index is out of bounds or cell is an obstacle
    if i >= len(obstacleGrid) || j >= len(obstacleGrid[0]) || obstacleGrid[i][j] == 1 {
        return 0
    }
    
    // If we have reached the bottom-right corner
    if i == len(obstacleGrid)-1 && j == len(obstacleGrid[0])-1 {
        return 1
    }
    
    // Move right and down
    return findPaths(i+1, j, obstacleGrid) + findPaths(i, j+1, obstacleGrid)
}
```

- **Time Complexity**: O(2^(m+n)) where m is the number of rows and n is the number of columns. Each cell has two choices.
- **Space Complexity**: O(m+n) due to recursive stack space.

---

### Memoization Approach

This approach uses a top-down recursive strategy with caching of results for overlapping subproblems.

#### Intuition
- Utilize a 2D slice to store the result of paths from each cell.
- If we've computed the paths from a cell, reuse the stored result.

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    m, n := len(obstacleGrid), len(obstacleGrid[0])
    memo := make([][]int, m)
    for i := range memo {
        memo[i] = make([]int, n)
        for j := range memo[i] {
            memo[i][j] = -1
        }
    }
    return findPathsMemo(0, 0, obstacleGrid, memo)
}

func findPathsMemo(i, j int, obstacleGrid [][]int, memo [][]int) int {
    // Base cases
    if i >= len(obstacleGrid) || j >= len(obstacleGrid[0]) || obstacleGrid[i][j] == 1 {
        return 0
    }
    if i == len(obstacleGrid)-1 && j == len(obstacleGrid[0])-1 {
        return 1
    }
    
    // Check if already computed
    if memo[i][j] != -1 {
        return memo[i][j]
    }
    
    // Cache the result
    memo[i][j] = findPathsMemo(i+1, j, obstacleGrid, memo) + findPathsMemo(i, j+1, obstacleGrid, memo)
    return memo[i][j]
}
```

- **Time Complexity**: O(m*n) because each cell's result is computed once.
- **Space Complexity**: O(m*n) for the memoization table.

---

### Dynamic Programming Approach

Use an iterative solution to compute the number of paths using a DP table.

#### Intuition
- Create a DP table where each cell represents the number of ways to reach that cell.
- Initialize the first row and first column considering obstacles.
- Update each cell in the DP table based on the sum of ways from the top and left cells.

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
    m, n := len(obstacleGrid), len(obstacleGrid[0])
    if obstacleGrid[0][0] == 1 {
        return 0
    }
    
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    
    // Start point
    dp[0][0] = 1
    
    // Fill the first column
    for i := 1; i < m; i++ {
        if obstacleGrid[i][0] == 0 {
            dp[i][0] = dp[i-1][0]
        }
    }
    
    // Fill the first row
    for j := 1; j < n; j++ {
        if obstacleGrid[0][j] == 0 {
            dp[0][j] = dp[0][j-1]
        }
    }
    
    // Calculate paths for the rest of the grid
    for i := 1; i < m; i++ {
        for j := 1; j < n; j++ {
            if obstacleGrid[i][j] == 0 {
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
            }
        }
    }
    
    return dp[m-1][n-1]
}
```

- **Time Complexity**: O(m*n) since we iterate through the grid once.
- **Space Complexity**: O(m*n) for the DP table.

This method is optimal in terms of both time and space efficiency for handling obstacles by leveraging dynamic programming.

