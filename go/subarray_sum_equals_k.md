# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func subarraySum(nums []int, k int) int {
    count := 0

    // Check all subarrays
    for i := 0; i < len(nums); i++ {
        sum := 0

        for j := i; j < len(nums); j++ {
            sum += nums[j]

            // If the sum equals k, increment the count
            if sum == k {
                count++
            }
        }
    }

    return count
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func subarraySum(nums []int, k int) int {
    prefixSumMap := make(map[int]int)
    prefixSumMap[0] = 1 // Initialize with a prefix sum of 0

    count, sum := 0, 0

    for _, num := range nums {
        sum += num // Update the prefix sum

        // Check if there's a prefix sum that makes the current sum equal to k
        if val, found := prefixSumMap[sum-k]; found {
            count += val
        }

        // Update the frequency of the current prefix sum
        prefixSumMap[sum]++
    }

    return count
}
```

