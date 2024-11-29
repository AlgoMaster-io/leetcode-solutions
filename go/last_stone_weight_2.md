# 1049. [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)

## Approach 1: Dynamic Programming with a Set

### Solution
go
```go
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
package main

import "math"

func lastStoneWeightII(stones []int) int {
    dp := make(map[int]bool)
    dp[0] = true
    sum := 0

    // Iterate over each stone
    for _, stone := range stones {
        newDp := make(map[int]bool)
        // Update possible sums with and without the current stone
        for s := range dp {
            newDp[s+stone] = true
            newDp[s-stone] = true
        }
        // Move to the new set of possible sums
        dp = newDp
        sum += stone
    }

    // Find the minimum possible non-negative sum (half the total sum)
    minAbs := math.MaxInt32
    for s := range dp {
        if s >= 0 {
            if s < minAbs {
                minAbs = s
            }
        }
    }

    return minAbs
}
```

## Approach 2: Dynamic Programming with Boolean Array

### Solution
go
```go
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
package main

func lastStoneWeightII(stones []int) int {
    sum := 0
    for _, stone := range stones {
        sum += stone
    }
    dp := make([]bool, sum/2+1)
    dp[0] = true

    // Evaluate possible subset sums
    for _, stone := range stones {
        for j := len(dp) - 1; j >= stone; j-- {
            dp[j] = dp[j] || dp[j-stone]
        }
    }

    // Find the largest possible subset sum that is <= sum/2
    for j := len(dp) - 1; j >= 0; j-- {
        if dp[j] {
            return sum - 2*j
        }
    }

    return 0
}
```

## Approach 3: Optimized DP with Single Array

### Solution
go
```go
// Time Complexity: O(n * S), where S is the sum of all stones
// Space Complexity: O(S)
package main

func lastStoneWeightII(stones []int) int {
    sum := 0
    for _, stone := range stones {
        sum += stone
    }
    target := sum / 2
    dp := make([]int, target+1)

    // Calculate optimized subset sums
    for _, stone := range stones {
        for j := target; j >= stone; j-- {
            dp[j] = max(dp[j], dp[j-stone]+stone)
        }
    }

    // Result is the difference between total sum and twice the best subset sum
    return sum - 2*dp[target]
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

