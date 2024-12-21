# [Leetcode 312: Burst Balloons](https://leetcode.com/problems/burst-balloons/)

- [Approach 1: Brute Force Recursion](#approach-1-brute-force-recursion)
- [Approach 2: Memoization (Top-Down Dynamic Programming)](#approach-2-memoization)
- [Approach 3: Tabulation (Bottom-Up Dynamic Programming)](#approach-3-tabulation)

## Approach 1: Brute Force Recursion

### Intuition:
The problem of bursting balloons can be broken down into subproblems where at each step, we consider bursting a single balloon, which divides the remaining balloons into two subproblems. The goal is to try all possible ways of picking balloons and return the one that gives us the maximum coins.

### Solution:
We use a recursive function to represent the process of choosing a balloon to burst, calculating the coins obtained by bursting that particular balloon, and plus the maximum coins obtainable from the balloons on the left and right of the chosen balloon. The base case is when there are no balloons left, corresponding to a burst operation on an empty set of balloons.

### Complexity:
- **Time Complexity:** O(n!), where n is the number of balloons. The number of possible ways to burst balloons grows factorially with the number of balloons.
- **Space Complexity:** O(n) due to the recursive call stack.

```go
func maxCoinsBF(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    
    // Add imaginary bounding balloons with value 1
    newNums := make([]int, n+2)
    copy(newNums[1:], nums)
    newNums[0], newNums[n+1] = 1, 1
    
    // Recursive function to find max coins
    var helper func(left, right int) int
    helper = func(left, right int) int {
        if left + 1 == right {
            return 0
        }
        
        maxCoins := 0
        // Choose the balloon to burst between left and right
        for i := left + 1; i < right; i++ {
            // Calculate coins obtained
            coins := newNums[left] * newNums[i] * newNums[right]
            // Calculate total coins by bursting i last
            totalCoins := coins + helper(left, i) + helper(i, right)
            maxCoins = max(maxCoins, totalCoins)
        }
        
        return maxCoins
    }
    
    return helper(0, n+1)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Memoization

### Intuition:
This approach optimizes the brute force approach by storing already computed results of subproblems, thereby avoiding redundant calculations. We utilize a memoization table to store the results of subproblem computations.

### Solution:
Modify the recursive solution to check the memoization table for precomputed results before computing a new one.

### Complexity:
- **Time Complexity:** O(n^3), because we solve each subproblem only once and there are O(n^2) subproblems.
- **Space Complexity:** O(n^2) for the memoization table.

```go
func maxCoinsMemo(nums []int) int {
    n := len(nums)
    newNums := make([]int, n+2)
    copy(newNums[1:], nums)
    newNums[0], newNums[n+1] = 1, 1
    
    memo := make([][]int, n+2)
    for i := range memo {
        memo[i] = make([]int, n+2)
    }
    
    var helper func(left, right int) int
    helper = func(left, right int) int {
        if left + 1 == right {
            return 0
        }
        
        if memo[left][right] > 0 {
            return memo[left][right]
        }
        
        maxCoins := 0
        for i := left + 1; i < right; i++ {
            coins := newNums[left] * newNums[i] * newNums[right]
            totalCoins := coins + helper(left, i) + helper(i, right)
            maxCoins = max(maxCoins, totalCoins)
        }
        
        memo[left][right] = maxCoins
        return maxCoins
    }
    
    return helper(0, n+1)
}
```

## Approach 3: Tabulation

### Intuition:
Using bottom-up dynamic programming, we avoid recursion altogether by iteratively solving each subproblem and filling a DP table. This approach ensures that each subproblem is only solved once.

### Solution:
Construct a 2D table where dp[left][right] represents the maximum coins that can be obtained by bursting all balloons between `left` and `right` exclusive. Fill this table in a manner where we solve smaller subproblems before larger ones.

### Complexity:
- **Time Complexity:** O(n^3) for iteratively filling the DP table.
- **Space Complexity:** O(n^2) for the DP table.

```go
func maxCoinsDP(nums []int) int {
    n := len(nums)
    newNums := make([]int, n+2)
    copy(newNums[1:], nums)
    newNums[0], newNums[n+1] = 1, 1
    
    dp := make([][]int, n+2)
    for i := range dp {
        dp[i] = make([]int, n+2)
    }
    
    // Length of the subproblem
    for length := 2; length < n+2; length++ {
        for left := 0; left < n+2-length; left++ {
            right := left + length
            for i := left + 1; i < right; i++ {
                dp[left][right] = max(dp[left][right], newNums[left]*newNums[i]*newNums[right] + dp[left][i] + dp[i][right])
            }
        }
    }
    
    return dp[0][n+1]
}
```

In all approaches, the fundamental idea is to exploit the multiplicative nature of this problem by using dynamic selections in order to gain maximum coins. The progression from naive recursive to memoized and finally dynamic programming solutions shows increasing levels of optimization and complexity management.

