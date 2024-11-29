# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

import "math"

func minSubArrayLen(target int, nums []int) int {
    minLength := math.MaxInt32

    // Iterate over all possible starting points
    for i := 0; i < len(nums); i++ {
        sum := 0

        // Iterate over all possible ending points
        for j := i; j < len(nums); j++ {
            sum += nums[j]

            // Check if the sum is greater than or equal to the target
            if sum >= target {
                if (j - i + 1) < minLength {
                    minLength = j - i + 1
                }
                break // Break as the subarray length cannot decrease further
            }
        }
    }

    if minLength == math.MaxInt32 {
        return 0
    }
    return minLength
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "math"

func minSubArrayLen(target int, nums []int) int {
    left := 0
    sum := 0
    minLength := math.MaxInt32

    // Sliding window
    for right := 0; right < len(nums); right++ {
        sum += nums[right]

        // Shrink the window until the sum is less than the target
        for sum >= target {
            if (right - left + 1) < minLength {
                minLength = right - left + 1
            }
            sum -= nums[left]
            left++
        }
    }

    if minLength == math.MaxInt32 {
        return 0
    }
    return minLength
}
```

