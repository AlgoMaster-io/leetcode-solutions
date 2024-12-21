# [LeetCode 11: Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Two Pointers Approach](#two-pointers-approach)

### Brute Force Approach

The problem is to find two lines in an array that, together with the x-axis, form a container that holds the most water. The brute force solution involves checking every possible pair of lines and calculating the volume of water that can be stored.

#### Intuition
1. We need to calculate the area formed between every pair of lines.
2. The area between two lines is determined by the shorter line.
3. For each possible pair of lines, calculate the area and keep track of the maximum.

#### Time Complexity
- The time complexity is O(n^2) because we iterate over each pair of lines.

#### Space Complexity
- The space complexity is O(1) because we only store the maximum area found.

```go
func maxAreaBruteForce(height []int) int {
    maxArea := 0
    n := len(height)

    // Iterate over each pair of lines
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            // Calculate the width
            width := j - i
            // Calculate the area with the shorter line as height
            currentArea := min(height[i], height[j]) * width

            // Update maxArea if the current area is larger
            if currentArea > maxArea {
                maxArea = currentArea
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
```

### Two Pointers Approach

Instead of considering every possible pair of lines, we can use the two-pointer technique to efficiently find the maximum area.

#### Intuition
1. Start with two pointers, one at the beginning and one at the end of the array.
2. Calculate the area formed between the two pointers.
3. Move the pointer pointing to the shorter line inward, as moving the taller line will not increase the area.
4. Repeat until the two pointers meet.

This approach reduces the problem from O(n^2) to O(n) by using a greedy strategy to eliminate unnecessary pairs on each step.

#### Time Complexity
- The time complexity is O(n) because each pointer will be moved at most once through the array.

#### Space Complexity
- The space complexity is O(1) because it uses a constant amount of extra space.

```go
func maxAreaTwoPointers(height []int) int {
    maxArea := 0
    left := 0
    right := len(height) - 1

    for left < right {
        // Calculate the width
        width := right - left
        // Calculate the smaller of the two lines to get the height
        currentHeight := min(height[left], height[right])
        // Calculate the area
        currentArea := currentHeight * width
        
        if currentArea > maxArea {
            maxArea = currentArea
        }
        
        // Move the pointer pointing at the line with less height
        if height[left] < height[right] {
            left++
        } else {
            right--
        }
    }

    return maxArea
}
```

In the two-pointer approach, by cleverly holding a reference to the maximum constraints provided by the array (height) on both ends, and dynamically sliding towards the center depending on the lower of the lines, we efficiently maximize the area. This insight considerably reduces computational complexity from quadratic to linear.

