# [Leetcode 213: House Robber II](https://leetcode.com/problems/house-robber-ii/)

## Table of Contents
1. [Approach 1: Simple Recursive Solution](#approach-1-simple-recursive-solution)
2. [Approach 2: Dynamic Programming with Memoization](#approach-2-dynamic-programming-with-memoization)
3. [Approach 3: Dynamic Programming with Space Optimization](#approach-3-dynamic-programming-with-space-optimization)

### Approach 1: Simple Recursive Solution

**Intuition:**

The problem is a variation of the classic "House Robber" problem with the additional constraint that the houses are arranged in a circle. This means that the first and last house are adjacent, so robbing them both would result in an alert.

To solve this using a simple recursive approach, consider:
- Breaking the problem into two linear cases:
  1. Rob houses from 0 to n-2 (ignoring the last house).
  2. Rob houses from 1 to n-1 (ignoring the first house).
- For each case, use a recursive solution to determine the maximum amount that can be robbed.

**Implementation:**

```go
func robRecursive(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }
    // Two cases: Include the first house, exclude the last OR exclude the first, include the last
    return max(robHelper(nums, 0, len(nums)-2), robHelper(nums, 1, len(nums)-1))
}

func robHelper(nums []int, start, end int) int {
    if start > end {
        return 0
    }
    // Recursive step: either rob the current house and move two steps OR skip the house
    return max(nums[start] + robHelper(nums, start+2, end), robHelper(nums, start+1, end))
}

// Utility to find the maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Complexity Analysis:**
- **Time Complexity:** O(2^n) due to the exponential number of recursive calls.
- **Space Complexity:** O(n) for the recursive call stack.

### Approach 2: Dynamic Programming with Memoization

**Intuition:**

The recursive approach results in many overlapping subproblems. Use memoization to store the result of each subproblem to avoid redundant calculations.

**Implementation:**

```go
func robMemo(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }

    // Use memoization array for storing already computed results for two cases.
    memo1 := make([]int, len(nums))
    memo2 := make([]int, len(nums))
    for i := range memo1 {
        memo1[i] = -1
        memo2[i] = -1
    }

    // Max of both scenarios: excluding the first or excluding the last house
    return max(robMemoHelper(nums, 0, len(nums)-2, memo1), robMemoHelper(nums, 1, len(nums)-1, memo2))
}

func robMemoHelper(nums []int, start, end int, memo []int) int {
    if start > end {
        return 0
    }
    if memo[start] != -1 {
        return memo[start]
    }
    memo[start] = max(nums[start] + robMemoHelper(nums, start+2, end, memo), robMemoHelper(nums, start+1, end, memo))
    return memo[start]
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n) due to memoization reducing redundant calls.
- **Space Complexity:** O(n) for the memoization array and call stack.

### Approach 3: Dynamic Programming with Space Optimization

**Intuition:**

Optimize space by maintaining only two variables instead of an entire memoization array. This works due to the nature of the recurrence relation which only needs results from the two previous steps.

**Implementation:**

```go
func rob(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    if len(nums) == 1 {
        return nums[0]
    }
    return max(robLinear(nums, 0, len(nums)-2), robLinear(nums, 1, len(nums)-1))
}

func robLinear(nums []int, start, end int) int {
    prev1, prev2 := 0, 0
    for i := start; i <= end; i++ {
        temp := prev1
        prev1 = max(prev2 + nums[i], prev1) // Choose between robbing current house or not
        prev2 = temp
    }
    return prev1
}
```

**Complexity Analysis:**
- **Time Complexity:** O(n) as it iterates through the list twice.
- **Space Complexity:** O(1) since only a fixed amount of extra space is used.

