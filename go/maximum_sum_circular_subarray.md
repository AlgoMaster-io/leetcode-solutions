# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

import "math"

func maxSubarraySumCircular(nums []int) int {
    n := len(nums)
    maxSum := math.MinInt32

    // Iterate over all possible subarrays, including circular ones
    for i := 0; i < n; i++ {
        currentSum := 0

        for j := 0; j < n; j++ {
            currentSum += nums[(i+j)%n]
            maxSum = max(maxSum, currentSum)
        }
    }

    return maxSum
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Optimized with Kadane’s Algorithm

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func maxSubarraySumCircular(nums []int) int {
    totalSum, maxSum, currentMax := 0, nums[0], 0
    minSum, currentMin := nums[0], 0

    for _, num := range nums {
        // Calculate maximum subarray sum using Kadane's algorithm
        currentMax = max(num, currentMax+num)
        maxSum = max(maxSum, currentMax)

        // Calculate minimum subarray sum using Kadane's algorithm
        currentMin = min(num, currentMin+num)
        minSum = min(minSum, currentMin)

        // Calculate total sum of the array
        totalSum += num
    }

    // If all numbers are negative, return the maximum sum (non-circular)
    if maxSum > 0 {
        return max(maxSum, totalSum-minSum)
    }
    return maxSum
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

