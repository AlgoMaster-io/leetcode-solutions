# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n * k)
// Space Complexity: O(1)
package main

import "math"

func findMaxAverage(nums []int, k int) float64 {
    maxAverage := math.Inf(-1)

    // Check all subarrays of size k
    for i := 0; i <= len(nums)-k; i++ {
        sum := 0

        // Calculate the sum of the current subarray
        for j := i; j < i+k; j++ {
            sum += nums[j]
        }

        // Update the maximum average
        maxAverage = math.Max(maxAverage, float64(sum)/float64(k))
    }

    return maxAverage
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "math"

func findMaxAverage(nums []int, k int) float64 {
    sum := 0

    // Calculate the sum of the first window
    for i := 0; i < k; i++ {
        sum += nums[i]
    }

    maxAverage := float64(sum) / float64(k)

    // Slide the window over the array
    for i := k; i < len(nums); i++ {
        sum += nums[i] - nums[i-k] // Update the sum for the new window
        maxAverage = math.Max(maxAverage, float64(sum)/float64(k)) // Update the maximum average
    }

    return maxAverage
}
```

