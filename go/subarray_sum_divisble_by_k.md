# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func subarraysDivByK(nums []int, k int) int {
    count := 0

    // Check all subarrays
    for i := 0; i < len(nums); i++ {
        sum := 0
        for j := i; j < len(nums); j++ {
            sum += nums[j]

            // Check if the sum is divisible by k
            if sum%k == 0 {
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
// Space Complexity: O(k)
package main

func subarraysDivByK(nums []int, k int) int {
    remainderMap := make(map[int]int)
    remainderMap[0] = 1 // Initialize with a remainder of 0

    count := 0
    sum := 0

    for _, num := range nums {
        sum += num
        remainder := sum % k

        // Normalize the remainder to always be non-negative
        if remainder < 0 {
            remainder += k
        }

        // If the remainder exists in the map, add its frequency to the count
        if freq, exists := remainderMap[remainder]; exists {
            count += freq
        }

        // Update the frequency of the current remainder
        remainderMap[remainder]++
    }

    return count
}
```

