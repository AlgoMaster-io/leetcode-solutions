# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approach 1: Brute Force (Basic)

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

func trap(height []int) int {
    n := len(height)
    totalWater := 0

    // Iterate over each bar
    for i := 1; i < n-1; i++ {
        leftMax, rightMax := 0, 0

        // Find the maximum height to the left of the current bar
        for j := 0; j <= i; j++ {
            if height[j] > leftMax {
                leftMax = height[j]
            }
        }

        // Find the maximum height to the right of the current bar
        for j := i; j < n; j++ {
            if height[j] > rightMax {
                rightMax = height[j]
            }
        }

        // Calculate water trapped at the current bar
        totalWater += min(leftMax, rightMax) - height[i]
    }

    return totalWater
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}
```

## Approach 2: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func trap(height []int) int {
    n := len(height)
    if n == 0 {
        return 0
    }

    leftMax := make([]int, n)
    rightMax := make([]int, n)
    totalWater := 0

    // Compute leftMax array
    leftMax[0] = height[0]
    for i := 1; i < n; i++ {
        leftMax[i] = max(leftMax[i-1], height[i])
    }

    // Compute rightMax array
    rightMax[n-1] = height[n-1]
    for i := n - 2; i >= 0; i-- {
        rightMax[i] = max(rightMax[i+1], height[i])
    }

    // Calculate water trapped at each bar
    for i := 0; i < n; i++ {
        totalWater += min(leftMax[i], rightMax[i]) - height[i]
    }

    return totalWater
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 3: Two Pointers (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

func trap(height []int) int {
    left, right := 0, len(height)-1
    leftMax, rightMax := 0, 0
    totalWater := 0

    // Use two pointers to calculate trapped water
    for left < right {
        if height[left] < height[right] {
            // Update leftMax and calculate water at left pointer
            if height[left] >= leftMax {
                leftMax = height[left]
            } else {
                totalWater += leftMax - height[left]
            }
            left++
        } else {
            // Update rightMax and calculate water at right pointer
            if height[right] >= rightMax {
                rightMax = height[right]
            } else {
                totalWater += rightMax - height[right]
            }
            right--
        }
    }

    return totalWater
}
```

