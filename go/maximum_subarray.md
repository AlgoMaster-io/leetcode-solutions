# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
func maxSubArray(nums []int) int {
    maxSum := math.MinInt64

    // Check all possible subarrays
    for i := 0; i < len(nums); i++ {
        currentSum := 0

        for j := i; j < len(nums); j++ {
            currentSum += nums[j]
            if currentSum > maxSum {
                maxSum = currentSum
            }
        }
    }

    return maxSum
}
```

## Approach 2: Kadane’s Algorithm (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
func maxSubArray(nums []int) int {
    maxSum := nums[0]
    currentSum := nums[0]

    // Traverse the array
    for i := 1; i < len(nums); i++ {
        // Update the current sum: either extend the subarray or start a new one
        currentSum = max(nums[i], currentSum + nums[i])

        // Update the maximum sum encountered so far
        maxSum = max(maxSum, currentSum)
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

