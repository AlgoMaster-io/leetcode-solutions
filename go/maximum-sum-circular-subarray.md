# [LeetCode 918: Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approaches
1. [Kadane's Algorithm](#kadanes-algorithm)
2. [Kadane's Algorithm with Circular Sum](#kadanes-algorithm-with-circular-sum)

### Kadane's Algorithm

Kadane's algorithm is a classic solution to find the maximum sum subarray in a non-circular array. The intuition is to iterate through the array, maintaining a running sum, and update the maximum sum found so far. If the running sum becomes negative, we reset it to zero since a negative sum would not contribute to maximizing future subarray sums.

#### Solution
The solution simply involves iterating through the array, updating our running sum and checking if we've found a new maximum sum.

```go
func maxSubarraySumCircular(nums []int) int {
    n := len(nums)
    maxSubarraySum := nums[0]
    currentMax := nums[0]
    
    // Standard Kadane's algorithm to find the maximum non-circular subarray sum
    for i := 1; i < n; i++ {
        currentMax = max(nums[i], currentMax + nums[i])
        maxSubarraySum = max(maxSubarraySum, currentMax)
    }
    
    return maxSubarraySum
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

### Kadane's Algorithm with Circular Sum

To handle the circular nature of the array, we need to consider the possibility that the subarray with the maximum sum might wrap around the end of the array. The intuition is twofold:
- Calculate the maximum subarray sum using the standard Kadane's algorithm.
- Calculate the minimum subarray sum using a variation of Kadane's. Subtract this from the total sum to get another possible maximum sum (the circular case).

If the entire array sums to the minimum subarray sum, it means all numbers are negative, and the non-circular result should be returned.

#### Solution

We extend the basic Kadane's to calculate both the maximum and the minimum subarray sums, and use the total sum of the array to evaluate the wrapped maximum sum.

```go
func maxSubarraySumCircular(nums []int) int {
    n := len(nums)
    
    totalSum := 0
    maxSubarraySum := nums[0]
    currentMax := nums[0]
    minSubarraySum := nums[0]
    currentMin := nums[0]
    
    for i := 0; i < n; i++ {
        totalSum += nums[i]
        
        // Standard Kadane's for max subarray
        currentMax = max(nums[i], currentMax + nums[i])
        maxSubarraySum = max(maxSubarraySum, currentMax)
        
        // Kadane's for min subarray
        currentMin = min(nums[i], currentMin + nums[i])
        minSubarraySum = min(minSubarraySum, currentMin)
    }
    
    // If all numbers are negative, maxSubarraySum would be the least negative number
    if maxSubarraySum < 0 {
        return maxSubarraySum
    }
    
    // Otherwise, consider circular sum as well
    return max(maxSubarraySum, totalSum - minSubarraySum)
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

