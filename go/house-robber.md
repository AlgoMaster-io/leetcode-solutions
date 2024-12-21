# [Leetcode 198: House Robber](https://leetcode.com/problems/house-robber/)

## Approaches:
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)
- [Approach 3: Dynamic Programming with Constant Space](#approach-3-dynamic-programming-with-constant-space)

## Approach 1: Recursion with Memoization

### Intuition
The problem follows the principle of making a decision at each house: rob it or not rob it based on the optimal solution of the previous houses. For each house, we have two choices:
1. Rob the current house and add its value to the maximum value obtained from the next non-adjacent house.
2. Skip the current house and take the maximum value obtained from the next house.

To optimize recursive solutions, we can use memoization to store intermediate results, thereby avoiding repeated calculations.

### Code
```go
func rob(nums []int) int {
    n := len(nums)
    memo := make(map[int]int)
    return robFrom(0, nums, memo)
}

func robFrom(index int, nums []int, memo map[int]int) int {
    // Base Case: When index reaches beyond the last house
    if index >= len(nums) {
        return 0
    }

    // Check if we already computed the result for current index
    if val, exists := memo[index]; exists {
        return val
    }

    // Rob the current house and move to the house after next
    robCurrent := nums[index] + robFrom(index+2, nums, memo)
    // Skip the current house
    skipCurrent := robFrom(index+1, nums, memo)

    // Store the maximum result in memo for current index
    result := max(robCurrent, skipCurrent)
    memo[index] = result
    return result
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of houses, as each house index is computed once and stored.
- **Space Complexity**: O(n), used for storing memoization results.

## Approach 2: Dynamic Programming (Bottom-Up)

### Intuition
Instead of solving the problem recursively from the front and using memoization, we can use a bottom-up dynamic programming approach. We keep track of the maximum amount of money that can be robbed up to the ith house using an array `dp`, where `dp[i]` indicates the maximum money that can be robbed from the first i houses.

### Code
```go
func rob(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    if n == 1 {
        return nums[0]
    }

    dp := make([]int, n)
    dp[0] = nums[0]
    dp[1] = max(nums[0], nums[1])

    for i := 2; i < n; i++ {
        // Either we take the current house and last non-adjacent best, or
        // we ignore the current house and take the last best
        dp[i] = max(nums[i]+dp[i-2], dp[i-1])
    }

    return dp[n-1]
}
```

### Complexity Analysis
- **Time Complexity**: O(n), as we iterate through each house once.
- **Space Complexity**: O(n), used for storing the dp array.

## Approach 3: Dynamic Programming with Constant Space

### Intuition
By noticing that in our dynamic programming array, we only ever use the last two values, we can optimize the space used from O(n) to O(1) by keeping just two variables to store the required values.

### Code
```go
func rob(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }
    if n == 1 {
        return nums[0]
    }
    
    prev2, prev1 := 0, nums[0]

    for i := 1; i < n; i++ {
        // Calculate the maximum value for the current house
        current := max(nums[i]+prev2, prev1)
        // Update prev2 and prev1 to move to the next house
        prev2 = prev1
        prev1 = current
    }

    return prev1
}
```

### Complexity Analysis
- **Time Complexity**: O(n), as we iterate through the list once.
- **Space Complexity**: O(1), as we are only using a fixed amount of variables.

