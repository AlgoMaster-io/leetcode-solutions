# [Leetcode Problem - 64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

## Approaches:
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)
- [Approach 3: Dynamic Programming with Tabulation](#approach-3-dynamic-programming-with-tabulation)
- [Approach 4: Dynamic Programming with Space Optimization](#approach-4-dynamic-programming-with-space-optimization)

## Approach 1: Recursive Solution

### Intuition:
The simplest method to solve this problem is using a recursive solution. The idea is to start from the top-left corner of the grid and explore all possible paths to the bottom-right corner, keeping track of the minimum path sum encountered.

The base case is when we reach the bottom-right corner of the grid, we simply return its value.

The recursive case is to move right or down from the current cell (i, j) to the cell (i, j+1) or (i+1, j), and accumulate the path sums, then take the minimum of both paths.

### Code:
```go
func minPathSum(grid [][]int) int {
    return recursive(grid, 0, 0)
}

func recursive(grid [][]int, row int, col int) int {
    // Base case: when we reach the bottom-right corner
    if row == len(grid)-1 && col == len(grid[0])-1 {
        return grid[row][col]
    }

    // If we reach the last row, we can only move right
    if row == len(grid)-1 {
        return grid[row][col] + recursive(grid, row, col+1)
    }

    // If we reach the last column, we can only move down
    if col == len(grid[0])-1 {
        return grid[row][col] + recursive(grid, row+1, col)
    }

    // Recursive call: choose the minimum path sum from right or down
    return grid[row][col] + min(recursive(grid, row+1, col), recursive(grid, row, col+1))
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Time Complexity:
- O(2^(m+n)): as each cell has two choices (right and down), it leads to exponential growth.

### Space Complexity:
- O(m+n): where m = number of rows and n = number of columns, due to the recursion stack.

---

## Approach 2: Dynamic Programming with Memoization

### Intuition:
To improve the efficiency of the recursive approach, we can use memoization to store the results of subproblems. This avoids redundant calculations and significantly reduces the time complexity.

### Code:
```go
func minPathSum(grid [][]int) int {
    memo := make(map[[2]int]int)
    return recursiveWithMemo(grid, 0, 0, memo)
}

func recursiveWithMemo(grid [][]int, row int, col int, memo map[[2]int]int) int {
    key := [2]int{row, col}
    if val, exists := memo[key]; exists {
        return val
    }

    // Base case: when we reach the bottom-right corner
    if row == len(grid)-1 && col == len(grid[0])-1 {
        return grid[row][col]
    }

    // If we reach the last row, we can only move right
    if row == len(grid)-1 {
        memo[key] = grid[row][col] + recursiveWithMemo(grid, row, col+1, memo)
        return memo[key]
    }

    // If we reach the last column, we can only move down
    if col == len(grid[0])-1 {
        memo[key] = grid[row][col] + recursiveWithMemo(grid, row+1, col, memo)
        return memo[key]
    }

    // Recursive call: choose the minimum path sum from right or down
    memo[key] = grid[row][col] + min(recursiveWithMemo(grid, row+1, col, memo), recursiveWithMemo(grid, row, col+1, memo))
    return memo[key]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Time Complexity:
- O(m*n): each subproblem is solved only once.

### Space Complexity:
- O(m*n): space used by memoization table.

---

## Approach 3: Dynamic Programming with Tabulation

### Intuition:
Instead of using recursion and memoization, we can use a bottom-up dynamic programming solution. By building a DP table that starts from the bottom-right and calculates the path sums to the top-left. This avoids stack overhead from recursion.

### Code:
```go
func minPathSum(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    dp := make([][]int, m)
    for i := range dp {
        dp[i] = make([]int, n)
    }

    // Start from bottom-right
    dp[m-1][n-1] = grid[m-1][n-1]

    // Fill last column
    for i := m - 2; i >= 0; i-- {
        dp[i][n-1] = grid[i][n-1] + dp[i+1][n-1]
    }

    // Fill last row
    for j := n - 2; j >= 0; j-- {
        dp[m-1][j] = grid[m-1][j] + dp[m-1][j+1]
    }

    // Fill the rest of the dp table
    for i := m - 2; i >= 0; i-- {
        for j := n - 2; j >= 0; j-- {
            dp[i][j] = grid[i][j] + min(dp[i+1][j], dp[i][j+1])
        }
    }

    return dp[0][0]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Time Complexity:
- O(m*n): as we fill out every cell once with the best path sum.

### Space Complexity:
- O(m*n): for the dp table.

---

## Approach 4: Dynamic Programming with Space Optimization

### Intuition:
We can further optimize the space used by realizing that at any point, we only need information from the current row and the row below. Thus, we can use two arrays to store the necessary information instead of a full DP table.

### Code:
```go
func minPathSum(grid [][]int) int {
    m, n := len(grid), len(grid[0])
    dp := make([]int, n)

    // Fill the last cell
    dp[n-1] = grid[m-1][n-1]

    // Fill the last row
    for j := n - 2; j >= 0; j-- {
        dp[j] = grid[m-1][j] + dp[j+1]
    }

    // Fill the rest rows
    for i := m - 2; i >= 0; i-- {
        // Last column initially
        dp[n-1] += grid[i][n-1]
        
        // Fill the remaining cells
        for j := n - 2; j >= 0; j-- {
            dp[j] = grid[i][j] + min(dp[j], dp[j+1])
        }
    }

    return dp[0]
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

### Time Complexity:
- O(m*n): as we compute the path sum for each cell.

### Space Complexity:
- O(n): where n is the number of columns, to store the DP array for the current row.

