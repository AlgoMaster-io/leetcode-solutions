# 84. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Brute Force

### Solution
go
```go
// Time Complexity: O(n^2)
// Space Complexity: O(1)
func largestRectangleArea(heights []int) int {
    maxArea := 0

    // Check every pair of bars as the boundaries of the rectangle
    for i := 0; i < len(heights); i++ {
        minHeight := heights[i]

        for j := i; j < len(heights); j++ {
            // Update the minimum height in the current range
            minHeight = min(minHeight, heights[j])
            // Calculate the area and update maxArea if needed
            maxArea = max(maxArea, minHeight*(j-i+1))
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

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
func largestRectangleArea(heights []int) int {
    stack := make([]int, 0) // Stack stores indices
    maxArea := 0
    n := len(heights)

    for i := 0; i <= n; i++ {
        // Use 0 height as a sentinel value for the end of the array
        currentHeight := 0
        if i != n {
            currentHeight = heights[i]
        }

        // Process and calculate area for indices in the stack
        for len(stack) > 0 && currentHeight < heights[stack[len(stack)-1]] {
            height := heights[stack[len(stack)-1]] // Height of the rectangle
            stack = stack[:len(stack)-1] // Pop from stack
            width := i
            if len(stack) > 0 {
                width = i - stack[len(stack)-1] - 1 // Width of the rectangle
            }
            maxArea = max(maxArea, height*width) // Update maxArea
        }

        stack = append(stack, i) // Push the current index
    }

    return maxArea
}
```

## Approach 3: Divide and Conquer

### Solution
go
```go
// Time Complexity: O(n log n) on average, O(n^2) in the worst case
// Space Complexity: O(n) due to recursion
func largestRectangleArea(heights []int) int {
    return calculateArea(heights, 0, len(heights)-1)
}

func calculateArea(heights []int, start, end int) int {
    if start > end {
        return 0
    }

    // Find the index of the minimum height in the range
    minIndex := start
    for i := start; i <= end; i++ {
        if heights[i] < heights[minIndex] {
            minIndex = i
        }
    }

    // Calculate the maximum area by dividing the problem
    currentArea := heights[minIndex] * (end - start + 1)
    leftArea := calculateArea(heights, start, minIndex-1)
    rightArea := calculateArea(heights, minIndex+1, end)

    return max(currentArea, max(leftArea, rightArea))
}
```


