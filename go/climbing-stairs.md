# [Leetcode 70: Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approaches:
1. [Recursive Approach (Brute Force)](#recursive-approach)
2. [Memoization Approach (Top-Down Dynamic Programming)](#memoization-approach)
3. [Tabulation Approach (Bottom-Up Dynamic Programming)](#tabulation-approach)
4. [Fibonacci Number Approach (Constant Space)](#fibonacci-number-approach)

### Recursive Approach (Brute Force)
The problem of climbing stairs can be interpreted as trying to find the distinct ways to reach the nth step. In this recursive method, consider:

- At each step, you have two choices: move one step or move two steps.
- Define a function `climbStairs(n)` which denotes the number of ways to reach the nth step.
- The base cases are when `n == 0` (1 way) or `n == 1` (1 way to reach the first step).
  
The recurrence relation is: `climbStairs(n) = climbStairs(n-1) + climbStairs(n-2)`

```go
func climbStairs(n int) int {
    if n <= 1 {
        return 1
    }
    return climbStairs(n-1) + climbStairs(n-2)
}
```

- **Time Complexity:** O(2^n), as we calculate each state multiple times.
- **Space Complexity:** O(n), due to the recursive call stack.

### Memoization Approach (Top-Down Dynamic Programming)
This approach optimizes the recursive function by storing already calculated results:

- Use an array to store the number of ways to reach each step.
- If `dp[i]` is already calculated, return the stored value instead of recalculating.

```go
func climbStairs(n int) int {
    dp := make([]int, n+1)
    return climbStairsHelper(n, dp)
}

func climbStairsHelper(n int, dp []int) int {
    if n <= 1 {
        return 1
    }
    if dp[n] > 0 {
        return dp[n]
    }
    dp[n] = climbStairsHelper(n-1, dp) + climbStairsHelper(n-2, dp)
    return dp[n]
}
```

- **Time Complexity:** O(n)
- **Space Complexity:** O(n), due to the memoization array.

### Tabulation Approach (Bottom-Up Dynamic Programming)
The idea here is to build the solution from the ground up:

- Create a dp array where `dp[i]` indicates the number of ways to reach the ith step.
- Initialize base cases `dp[0] = 1` and `dp[1] = 1`.
- For each step from 2 to n, calculate `dp[i] = dp[i-1] + dp[i-2]`.

```go
func climbStairs(n int) int {
    if n <= 1 {
        return 1
    }
    dp := make([]int, n+1)
    dp[0], dp[1] = 1, 1
    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }
    return dp[n]
}
```

- **Time Complexity:** O(n)
- **Space Complexity:** O(n), due to the dp array.

### Fibonacci Number Approach (Constant Space)
This approach is based on the observation that the number of ways to reach the steps is similar to the Fibonacci numbers:

- Use two variables to store the previous two results and iteratively solve for the nth result.

```go
func climbStairs(n int) int {
    if n <= 1 {
        return 1
    }
    first, second := 1, 1
    for i := 2; i <= n; i++ {
        third := first + second // store sum of last two results
        first = second          // update first
        second = third          // update second
    }
    return second
}
```

- **Time Complexity:** O(n)
- **Space Complexity:** O(1), only a few variables are used. 

Each method improves upon the previous in terms of space or time complexity, with the final method being both optimal and efficient for large values of `n`.

