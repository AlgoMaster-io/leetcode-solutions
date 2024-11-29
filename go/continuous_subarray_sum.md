# 523. [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func checkSubarraySum(nums []int, k int) bool {
	n := len(nums)

	// Check every possible subarray
	for i := 0; i < n-1; i++ {
		sum := nums[i]
		for j := i + 1; j < n; j++ {
			sum += nums[j]
			if k == 0 {
				if sum == 0 {
					return true // Subarray sum is a multiple of k (zero case)
				}
			} else {
				if sum%k == 0 {
					return true // Subarray sum is a multiple of k
				}
			}
		}
	}
	return false // No valid subarray found
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func checkSubarraySum(nums []int, k int) bool {
	remainderMap := make(map[int]int)
	remainderMap[0] = -1 // Initialize with a remainder of 0 at index -1
	sum := 0

	for i, num := range nums {
		sum += num

		// Compute the remainder when divided by k
		remainder := 0
		if k != 0 {
			remainder = sum % k
		} else {
			remainder = sum
		}

		// If the remainder already exists
		if prevIndex, exists := remainderMap[remainder]; exists {
			// Check if the subarray length is at least 2
			if i-prevIndex > 1 {
				return true
			}
		} else {
			// Store the index of this remainder
			remainderMap[remainder] = i
		}
	}

	return false // No valid subarray found
}
```

