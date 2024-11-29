# 198. [House Robber](https://leetcode.com/problems/house-robber/)

## Approach 1: Recursive Solution

### Solution
```go
// Time Complexity: O(2^n)
// Space Complexity: O(n)
package main

import "math"

func rob(nums []int) int {
    return robFrom(0, nums)
}

func robFrom(index int, nums []int) int {
    // Base case: If index is out of bounds, return 0
    if index >= len(nums) {
        return 0
    }

    // Either rob this house and skip one, or skip it
    robCurrent := nums[index] + robFrom(index+2, nums)
    skipCurrent := robFrom(index+1, nums)

    // Return the maximum of both choices
    return int(math.Max(float64(robCurrent), float64(skipCurrent)))
}
```

## Approach 2: Recursion with Memoization

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func rob(nums []int) int {
    memo := make(map[int]int)
    return robFrom(0, nums, memo)
}

func robFrom(index int, nums []int, memo map[int]int) int {
    // Base case: If index is out of bounds, return 0
    if index >= len(nums) {
        return 0
    }

    // Check memoization table
    if val, exists := memo[index]; exists {
        return val
    }

    // Either rob this house and skip one, or skip it
    robCurrent := nums[index] + robFrom(index+2, nums, memo)
    skipCurrent := robFrom(index+1, nums, memo)

    // Memoize the result
    result := int(math.Max(float64(robCurrent), float64(skipCurrent)))
    memo[index] = result

    return result
}
```

## Approach 3: Dynamic Programming (Bottom Up)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func rob(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    dp := make([]int, len(nums)+1)
    dp[0] = 0
    dp[1] = nums[0]

    // Build the dp array
    for i := 2; i <= len(nums); i++ {
        dp[i] = int(math.Max(float64(dp[i-1]), float64(dp[i-2]+nums[i-1])))
    }

    return dp[len(nums)]
}
```

## Approach 4: Optimized Dynamic Programming (Constant Space)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func rob(nums []int) int {
    if len(nums) == 0 {
        return 0
    }

    prev1, prev2 := 0, 0

    // Iterate through each house
    for _, num := range nums {
        temp := prev1
        prev1 = int(math.Max(float64(prev2+num), float64(prev1)))
        prev2 = temp
    }

    return prev1
}
```


