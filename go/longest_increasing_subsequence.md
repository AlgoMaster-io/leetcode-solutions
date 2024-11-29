# 300. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approach 1: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(n)
func lengthOfLIS(nums []int) int {
    if nums == nil || len(nums) == 0 {
        return 0
    }
    
    dp := make([]int, len(nums)) // dp array to store the LIS ending at each index
    maxLength := 1 // Minimum LIS length is 1 (a single element)

    // Initialize all dp values to 1
    for i := range dp {
        dp[i] = 1
    }

    // Build the dp array
    for i := 1; i < len(nums); i++ {
        for j := 0; j < i; j++ {
            if nums[i] > nums[j] { // Check if nums[i] can extend the subsequence at nums[j]
                dp[i] = max(dp[i], dp[j] + 1) // Update dp[i] with the maximum length found
            }
        }
        maxLength = max(maxLength, dp[i]) // Update the overall LIS length
    }

    return maxLength
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Binary Search with Patience Sorting

### Solution
go
```go
// Time Complexity: O(n log n)
// Space Complexity: O(n)
func lengthOfLIS(nums []int) int {
    if nums == nil || len(nums) == 0 {
        return 0
    }

    tails := make([]int, len(nums)) // Array to store the smallest tail for all increasing subsequences
    size := 0 // Size of the effective array

    for _, num := range nums {
        i, j := 0, size // Binary search for the insertion point of num in tails

        for i < j {
            mid := (i + j) / 2
            if tails[mid] < num {
                i = mid + 1
            } else {
                j = mid
            }
        }

        tails[i] = num // Replace or extend the increasing subsequence
        if i == size {
            size++ // Increment size if a new subsequence tail is created
        }
    }

    return size
}
```

