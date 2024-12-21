# [Leetcode 120: Triangle](https://leetcode.com/problems/triangle/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization](#memoization)
3. [Dynamic Programming (Bottom-Up)](#dynamic-programming-bottom-up)
4. [Space Optimized Dynamic Programming](#space-optimized-dynamic-programming)

### Recursive Approach
The simplest approach to solve this problem is using recursion. Here, for each number in the triangle, recursively call the function to compute the minimum path down to the triangle from the current position. This involves two choices at each level - moving to the next row staying at the same index, and moving to the next row going to the next index.

#### Intuition:
This approach explores all possible paths, calculating the minimum path sum for each of them and finally returning the overall minimum path sum.

#### Code:
```go
func minimumTotal(triangle [][]int) int {
    return helper(triangle, 0, 0)
}

func helper(triangle [][]int, row, col int) int {
    // If we reach the bottom row, return the value at the current position.
    if row == len(triangle) { 
        return 0
    }
    // Calculate the minimum path sum from the current position by moving to next rows.
    down := helper(triangle, row+1, col)
    downRight := helper(triangle, row+1, col+1)
    return triangle[row][col] + min(down, downRight)
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

#### Time Complexity:
- `O(2^n)` where `n` is the number of rows in the triangle.

#### Space Complexity:
- `O(n)` for recursion stack.

### Memoization
To optimize the recursive solution, we can use memoization to store already computed results and avoid redundant calculations, thereby reducing the time complexity.

#### Intuition:
Store already computed results in a 2D array and use these results when needed instead of recalculating them.

#### Code:
```go
func minimumTotal(triangle [][]int) int {
    memo := make([][]int, len(triangle))
    for i := range memo {
        memo[i] = make([]int, len(triangle[i]))
        for j := range memo[i] {
            memo[i][j] = math.MinInt32
        }
    }
    return memoHelper(triangle, 0, 0, memo)
}

func memoHelper(triangle [][]int, row, col int, memo [][]int) int {
    if row == len(triangle) { 
        return 0
    }
    if memo[row][col] != math.MinInt32 {
        return memo[row][col]
    }
    
    down := memoHelper(triangle, row+1, col, memo)
    downRight := memoHelper(triangle, row+1, col+1, memo)
    
    memo[row][col] = triangle[row][col] + min(down, downRight)
    return memo[row][col]
}
```

#### Time Complexity:
- `O(n^2)` since we compute the result for each node once.

#### Space Complexity:
- `O(n^2)` for the memoization array.

### Dynamic Programming (Bottom-Up)
This approach involves solving the problem from bottom up, modifying the triangle in place to store the minimum path sum from the bottom to the top.

#### Intuition:
Start from the second last row and move upwards, for each element calculate and store the minimum path sum by considering the elements directly below.

#### Code:
```go
func minimumTotal(triangle [][]int) int {
    n := len(triangle)
    for row := n - 2; row >= 0; row-- {
        for col := 0; col <= row; col++ {
            triangle[row][col] += min(triangle[row+1][col], triangle[row+1][col+1])
        }
    }
    return triangle[0][0]
}
```

#### Time Complexity:
- `O(n^2)` where `n` is the number of rows in the triangle.

#### Space Complexity:
- `O(1)` since we modify the triangle in place.

### Space Optimized Dynamic Programming
We can further optimize space by keeping only a single row of computed minimum paths instead of modifying the triangle.

#### Intuition:
Instead of modifying the triangle to store results, use an auxiliary array that stores the minimum path sums from the already processed rows.

#### Code:
```go
func minimumTotal(triangle [][]int) int {
    n := len(triangle)
    dp := make([]int, n)
    copy(dp, triangle[n-1])

    for row := n - 2; row >= 0; row-- {
        for col := 0; col <= row; col++ {
            dp[col] = triangle[row][col] + min(dp[col], dp[col+1])
        }
    }
    return dp[0]
}
```

#### Time Complexity:
- `O(n^2)` where `n` is the number of rows in the triangle.

#### Space Complexity:
- `O(n)` for the dp array storing one row's minimum path sums.

