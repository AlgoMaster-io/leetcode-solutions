# [Problem: Leetcode 53 - Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

### Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Kadane's Algorithm](#kadane's-algorithm)

## Brute Force Approach

### Intuition
The brute force approach simply involves inspecting every possible subarray of the given array, calculating their sums, and keeping track of the maximum sum encountered. Though this method ensures we find the correct maximum, it is not efficient for larger arrays because it examines every possible subarray.

### Approach
- Use a nested loop to explore all subarrays, `nums[i:j]`.
- Calculate the sum for each subarray and keep track of the maximum sum seen so far in a variable `maxSum`.
- Return `maxSum` at the end, which will be the largest sum of any subarray.

### Code

```go
func maxSubArray(nums []int) int {
    n := len(nums)
    maxSum := nums[0] // Initialize to first element in case all are negative
    
    for i := 0; i < n; i++ {
        currentSum := 0
        for j := i; j < n; j++ {
            currentSum += nums[j] // Accumulate sum of current subarray
            
            if currentSum > maxSum {
                maxSum = currentSum // Update maxSum if currentSum is larger
            }
        }
    }
    
    return maxSum
}
```

### Complexity Analysis
- **Time Complexity**: O(n^2) - Two nested loops iterate through all n(n+1)/2 subarrays.
- **Space Complexity**: O(1) - Constant extra space used for variables.

## Kadane's Algorithm

### Intuition
Kadane's algorithm is an effective optimization over the brute force method. It is based on the idea of iterative computation; at each element, decide whether to include it in the current subarray or start a new subarray. This decision is made by comparing the current element to the sum of it and the current subarray sum. The goal is to keep track of the maximum sum encountered thus far.

### Approach
- Initialize two variables: `currentSum` (current subarray sum) and `maxSum` (global max subarray sum).
- Traverse the array:
  - At each element `nums[i]`, determine if it should start a new subarray by comparing `nums[i]` with `currentSum + nums[i]`.
  - Update `maxSum` with the greater of `maxSum` and `currentSum` after each iteration.
- Return `maxSum` which contains the maximum subarray sum found.

### Code

```go
func maxSubArray(nums []int) int {
    currentSum := nums[0]
    maxSum := nums[0]
    
    for i := 1; i < len(nums); i++ {
        // Decide to start a new subarray or extend the current one
        currentSum = max(nums[i], currentSum + nums[i])
        
        // Update max sum if the current subarray gives a larger sum
        maxSum = max(maxSum, currentSum)
    }
    
    return maxSum
}

// Helper function to get maximum of two numbers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity Analysis
- **Time Complexity**: O(n) - Though the array is iterated through once.
- **Space Complexity**: O(1) - Only a fixed number of integer variables are used.

This problem serves as a classic application of dynamic programming principles by utilizing a simple yet efficient iterative approach. Kadane's algorithm showcases the power of reducing complexity from quadratic to linear, making it suitable for large datasets.

