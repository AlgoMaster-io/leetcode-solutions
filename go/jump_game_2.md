# 45. [Jump Game II](https://leetcode.com/problems/jump-game-ii/)

## Approach: Greedy Algorithm

### Solution

```go
// Time Complexity: O(n), where n is the length of the array
// Space Complexity: O(1)
package main

func jump(nums []int) int {
    jumps := 0       // Number of jumps required
    currentEnd := 0  // Farthest point that can be reached with the current number of jumps
    farthest := 0    // Farthest point that can be reached from the current position

    for i := 0; i < len(nums)-1; i++ {
        farthest = max(farthest, i+nums[i]) // Update the farthest point reachable

        // If we reach the end of the current range
        if i == currentEnd {
            jumps++                 // Increment the jump count
            currentEnd = farthest   // Update the current range to the farthest point
        }
    }

    return jumps // Return the total number of jumps
}

// Helper function to get the maximum of two integers
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```



