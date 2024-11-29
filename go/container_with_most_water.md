# [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
package main

import "fmt"

func maxArea(height []int) int {
    maxArea := 0

    // Check all pairs of lines
    for i := 0; i < len(height); i++ {
        for j := i + 1; j < len(height); j++ {
            // Calculate the area for the current pair of lines
            area := min(height[i], height[j]) * (j - i)
            if area > maxArea {
                maxArea = area // Update max area if needed
            }
        }
    }

    return maxArea
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    fmt.Println(maxArea([]int{1,8,6,2,5,4,8,3,7})) // Output: 49
}
```

## Approach 2: Two Pointers (Optimal)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(1)
package main

import "fmt"

func maxArea(height []int) int {
    left, right := 0, len(height) - 1 // Initialize pointers
    maxArea := 0 // Initialize max area

    // Iterate until the two pointers meet
    for left < right {
        // Calculate the current area
        area := min(height[left], height[right]) * (right - left)
        if area > maxArea {
            maxArea = area // Update max area if needed
        }

        // Move the pointer pointing to the shorter line
        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }

    return maxArea
}

func min(a, b int) int {
    if a < b {
        return a
    }
    return b
}

func main() {
    fmt.Println(maxArea([]int{1,8,6,2,5,4,8,3,7})) // Output: 49
}
```

