# 494. [Target Sum](https://leetcode.com/problems/target-sum/)

## Approach 1: Recursive Brute Force

### Solution
```go
// Time Complexity: O(2^n)
// Space Complexity: O(n)
package main

func findTargetSumWays(nums []int, target int) int {
    return calculate(nums, 0, 0, target)
}

func calculate(nums []int, i, sum, target int) int {
    // Base case: when we've considered all numbers
    if i == len(nums) {
        // Return 1 if we've reached the target sum, 0 otherwise
        if sum == target {
            return 1
        }
        return 0
    }

    // Explore both adding and subtracting the current number
    return calculate(nums, i+1, sum+nums[i], target) + calculate(nums, i+1, sum-nums[i], target)
}
```

## Approach 2: Dynamic Programming with Memoization

### Solution
```go
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
package main

func findTargetSumWays(nums []int, target int) int {
    memo := make(map[string]int)
    return calculate(nums, 0, 0, target, memo)
}

func calculate(nums []int, i, sum, target int, memo map[string]int) int {
    key := fmt.Sprintf("%d,%d", i, sum)

    // If result is already calculated for this state, return it
    if val, exists := memo[key]; exists {
        return val
    }

    // Base case: when we've considered all numbers
    if i == len(nums) {
        if sum == target {
            return 1
        }
        return 0
    }

    // Explore both adding and subtracting the current number
    add := calculate(nums, i+1, sum+nums[i], target, memo)
    subtract := calculate(nums, i+1, sum-nums[i], target, memo)

    // Save result in memo
    memo[key] = add + subtract
    return memo[key]
}
```

## Approach 3: Dynamic Programming with Bottom-Up

### Solution
```go
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
package main

func findTargetSumWays(nums []int, target int) int {
    sum := 0

    // Calculate total sum of the array
    for _, num := range nums {
        sum += num
    }

    // If sum is smaller than target or (sum + target) is odd, return 0
    if abs(target) > sum || (sum+target)%2 != 0 {
        return 0
    }

    s := (sum + target) / 2

    // dp[i] represents the number of ways to get the sum i
    dp := make([]int, s+1)
    dp[0] = 1

    // Update dp array
    for _, num := range nums {
        for j := s; j >= num; j-- {
            dp[j] += dp[j-num]
        }
    }

    return dp[s]
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}
```


