# 416. [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Approach 1: Recursive Approach

### Solution
go
```go
// Time Complexity: O(2^n)
// Space Complexity: O(n)
package main

func canPartition(nums []int) bool {
    sum := 0
    for _, num := range nums {
        sum += num
    }
    // If the sum is odd, partitioning into equal subsets is not possible
    if sum%2 != 0 {
        return false
    }
    
    return canPartitionRecursive(nums, sum/2, 0)
}

func canPartitionRecursive(nums []int, target, index int) bool {
    // Base cases
    if target == 0 {
        return true
    }
    if target < 0 || index >= len(nums) {
        return false
    }
    
    // Recursive call to include or exclude the current element
    return canPartitionRecursive(nums, target-nums[index], index+1) || 
           canPartitionRecursive(nums, target, index+1)
}
```

## Approach 2: Dynamic Programming (DP) with 2D Array

### Solution
go
```go
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
package main

func canPartition(nums []int) bool {
    sum := 0
    for _, num := range nums {
        sum += num
    }
    // Sum is odd so return false
    if sum%2 != 0 {
        return false
    }
    target := sum / 2
    
    // Create a DP table to store previously computed results
    dp := make([][]bool, len(nums)+1)
    for i := range dp {
        dp[i] = make([]bool, target+1)
    }
    
    // Initialize first column, sum of 0 can be achieved without any elements
    for i := 0; i <= len(nums); i++ {
        dp[i][0] = true
    }
    
    for i := 1; i <= len(nums); i++ {
        for j := 1; j <= target; j++ {
            if j < nums[i-1] {
                dp[i][j] = dp[i-1][j]
            } else {
                dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i-1]]
            }
        }
    }
    
    return dp[len(nums)][target]
}
```

## Approach 3: Dynamic Programming (DP) with 1D Array

### Solution
go
```go
// Time Complexity: O(n * target)
// Space Complexity: O(target)
package main

func canPartition(nums []int) bool {
    sum := 0
    for _, num := range nums {
        sum += num
    }
    // If the sum is odd, partitioning into equal subsets is not possible
    if sum%2 != 0 {
        return false
    }
    target := sum / 2
    
    // Create a DP array to hold boolean values
    dp := make([]bool, target+1)
    dp[0] = true // Initial state; zero sum can always be achieved
    
    for _, num := range nums {
        // Traverse from target backward to avoid overwriting
        for j := target; j >= num; j-- {
            dp[j] = dp[j] || dp[j-num]
        }
    }
    
    return dp[target]
}
```

