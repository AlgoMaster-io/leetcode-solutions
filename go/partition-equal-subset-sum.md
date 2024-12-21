# [Leetcode 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

## Navigation
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Recursive with Memoization](#approach-2-recursive-with-memoization)
- [Approach 3: Dynamic Programming - Bottom Up](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Optimized Dynamic Programming - 1D Array](#approach-4-optimized-dynamic-programming-1d-array)

## Approach 1: Recursive Solution

### Intuition
The basic idea is to try to partition the array into two halves. We attempt to create one subset with a total sum of half the original array's sum by including or excluding each element.

If the array sum is odd, we can immediately return false, as it's not possible to partition it into two equal subsets.

### Code

```go
func canPartition(nums []int) bool {
    var sum int
    for _, num := range nums {
        sum += num
    }
    
    if sum%2 != 0 {
        return false
    }
    
    return canPartitionRec(nums, len(nums)-1, sum/2)
}

func canPartitionRec(nums []int, n int, sum int) bool {
    // Base cases
    if sum == 0 {
        return true
    }
    if n < 0 || sum < 0 {
        return false
    }
    
    // Try including the current number or not
    return canPartitionRec(nums, n-1, sum-nums[n]) || canPartitionRec(nums, n-1, sum)
}
```

### Complexity
- **Time Complexity:** O(2^n), where n is the number of elements in the array. This is because each element can either be included in the subset or not.
- **Space Complexity:** O(n), due to the recursion stack.

## Approach 2: Recursive with Memoization

### Intuition
To avoid recomputation, we use a memoization technique. The idea is to store the results of subproblems so that when the same state is encountered again, we don't recompute it.

### Code

```go
func canPartition(nums []int) bool {
    var sum int
    for _, num := range nums {
        sum += num
    }
    
    if sum%2 != 0 {
        return false
    }
    
    memo := make(map[[2]int]bool)
    return canPartitionMemo(nums, len(nums)-1, sum/2, memo)
}

func canPartitionMemo(nums []int, n int, sum int, memo map[[2]int]bool) bool {
    if sum == 0 {
        return true
    }
    if n < 0 || sum < 0 {
        return false
    }
    
    key := [2]int{n, sum}
    if result, exists := memo[key]; exists {
        return result
    }
    
    result := canPartitionMemo(nums, n-1, sum-nums[n], memo) || canPartitionMemo(nums, n-1, sum, memo)
    memo[key] = result
    return result
}
```

### Complexity
- **Time Complexity:** O(n * sum/2), where n is the number of elements and sum is the sum of elements.
- **Space Complexity:** O(n * sum/2), for the memoization table and recursion stack.

## Approach 3: Dynamic Programming - Bottom Up

### Intuition
We use a 2D DP array where `dp[i][j]` will store whether it's possible to achieve the sum `j` using the first `i` numbers.

### Code

```go
func canPartition(nums []int) bool {
    var sum int
    for _, num := range nums {
        sum += num
    }
    
    if sum%2 != 0 {
        return false
    }
    
    target := sum / 2
    dp := make([][]bool, len(nums)+1)
    for i := range dp {
        dp[i] = make([]bool, target+1)
    }
    
    for i := 0; i <= len(nums); i++ {
        dp[i][0] = true
    }
    
    for i := 1; i <= len(nums); i++ {
        for j := 1; j <= target; j++ {
            if nums[i-1] <= j {
                dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i-1]]
            } else {
                dp[i][j] = dp[i-1][j]
            }
        }
    }
    
    return dp[len(nums)][target]
}
```

### Complexity
- **Time Complexity:** O(n * sum/2), where n is the number of elements.
- **Space Complexity:** O(n * sum/2), for the DP table.

## Approach 4: Optimized Dynamic Programming - 1D Array

### Intuition
This approach reduces space complexity by using a 1D array and updating it in a reverse order to prevent overwriting the values needed for current computation.

### Code

```go
func canPartition(nums []int) bool {
    var sum int
    for _, num := range nums {
        sum += num
    }
    
    if sum%2 != 0 {
        return false
    }
    
    target := sum / 2
    dp := make([]bool, target+1)
    dp[0] = true
    
    for _, num := range nums {
        for j := target; j >= num; j-- {
            dp[j] = dp[j] || dp[j-num]
        }
    }
    
    return dp[target]
}
```

### Complexity
- **Time Complexity:** O(n * sum/2), where n is the number of elements.
- **Space Complexity:** O(sum/2), due to the 1D array used.

