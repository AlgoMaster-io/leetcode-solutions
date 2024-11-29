# [2461. Maximum Sum of Distinct Subarrays With Length K](https://leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/)

## Approach 1: Brute Force (Basic)

### Solution
```go
// Time Complexity: O(n * k)
// Space Complexity: O(k)
package main

import "math"

func maximumSubarraySum(nums []int, k int) int {
    maxSum := 0

    // Iterate over every subarray of size k
    for i := 0; i <= len(nums)-k; i++ {
        uniqueElements := make(map[int]bool)
        sum := 0
        isValid := true

        for j := i; j < i+k; j++ {
            if uniqueElements[nums[j]] {
                isValid = false // Subarray contains duplicate
                break
            }
            uniqueElements[nums[j]] = true
            sum += nums[j]
        }

        if isValid {
            maxSum = int(math.Max(float64(maxSum), float64(sum)))
        }
    }

    return maxSum
}
```

## Approach 2: Sliding Window with HashMap (Optimal)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(k)
package main

func maximumSubarraySum(nums []int, k int) int {
    maxSum, currentSum := 0, 0
    frequencyMap := make(map[int]int)
    left := 0

    // Sliding window
    for right := 0; right < len(nums); right++ {
        currentSum += nums[right]
        frequencyMap[nums[right]]++

        // If the window size exceeds k, shrink it
        if right-left+1 > k {
            currentSum -= nums[left]
            frequencyMap[nums[left]]--
            if frequencyMap[nums[left]] == 0 {
                delete(frequencyMap, nums[left])
            }
            left++
        }

        // Check if the current window is valid (distinct elements)
        if right-left+1 == k && len(frequencyMap) == k {
            if currentSum > maxSum {
                maxSum = currentSum
            }
        }
    }

    return maxSum
}
```

