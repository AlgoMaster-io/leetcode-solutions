# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

In this problem, you are asked to find the maximum average of a contiguous subarray of length `k` from a given array of integers `nums`. 

You can solve this problem using different approaches:

- [Brute Force Approach](#brute-force-approach)
- [Sliding Window Approach](#sliding-window-approach)

## Brute Force Approach

### Intuition

The brute force approach involves calculating the average of every possible contiguous subarray of length `k` and then finding the maximum among these averages. This approach is straightforward but not efficient for larger input sizes.

### Approach

1. Initialize a variable `maxSum` with the minimum possible value.
2. Iterate over each starting index of the subarrays.
3. For each starting index, calculate the sum of the subarray of length `k`.
4. Update the `maxSum` if the current subarray sum is greater.
5. Return `maxSum / k` as the result.

### Code

```go
func findMaxAverage(nums []int, k int) float64 {
    maxSum := int(^uint(0) >> 1)  // Initialize to minimum integer.
    for i := 0; i <= len(nums) - k; i++ {
        currentSum := 0
        for j := 0; j < k; j++ {
            currentSum += nums[i + j]
        }
        if currentSum > maxSum {
            maxSum = currentSum
        }
    }
    return float64(maxSum) / float64(k)
}
```

### Time Complexity

- **Time Complexity**: O(n * k), where `n` is the number of elements in `nums`.
- **Space Complexity**: O(1), as no extra space is used beyond a few variables.

## Sliding Window Approach

### Intuition

The sliding window approach optimizes the brute force method by avoiding repeated calculations for overlapping parts of subarrays. By reusing the sum of the previous window and making a small update, this method improves efficiency.

### Approach

1. Calculate the initial sum of the first `k` elements.
2. Iterate through the array starting from the `k-th` element.
3. Slide the window by removing the first element of the previous window and adding the next element of the current window.
4. Keep track of the maximum sum encountered.
5. Return `maxSum / k` as the result.

### Code

```go
func findMaxAverage(nums []int, k int) float64 {
    currentSum := 0
    for i := 0; i < k; i++ {
        currentSum += nums[i]
    }
    
    maxSum := currentSum
    for i := k; i < len(nums); i++ {
        currentSum = currentSum - nums[i - k] + nums[i]  // Slide the window
        if currentSum > maxSum {
            maxSum = currentSum
        }
    }
    
    return float64(maxSum) / float64(k)
}
```

### Time Complexity 

- **Time Complexity**: O(n), where `n` is the number of elements in `nums`, since we iterate through `nums` just once.
- **Space Complexity**: O(1), as the space required does not depend on the input size.

By using the sliding window approach, we achieve a much more efficient solution with linear time complexity, making it suitable even for larger input sizes.

