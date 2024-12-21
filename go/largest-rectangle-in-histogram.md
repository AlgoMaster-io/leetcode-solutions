# [Leetcode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Divide and Conquer Approach](#divide-and-conquer-approach)
- [Stack-Based Optimal Approach](#stack-based-optimal-approach)

---

### Brute Force Approach

#### Intuition

The brute force approach examines every possible rectangle in the histogram by considering each bar as the shortest bar in a potential rectangle and then determining the maximum width possible for each. This involves two nested loops: the outer loop selects the starting bar, and the inner loop expands towards the right while maintaining the rule that all heights in the rectangle are at least as tall as the starting bar. This is a simple approach, but inefficient for large histograms as it examines every possible combination.

#### Go Code
```go
func largestRectangleAreaBruteForce(heights []int) int {
    maxArea := 0
    n := len(heights)
    
    // Iterate over every bar for the width we can use
    for i := 0; i < n; i++ {
        minHeight := heights[i]
        
        // Expand the rectangle to the right
        for j := i; j < n; j++ {
            // Ensure all bars are at least as tall as heights[i]
            if heights[j] < minHeight {
                minHeight = heights[j]
            }
            // Calculate area with heights[i] as the shortest bar
            currentArea := minHeight * (j - i + 1)
            if currentArea > maxArea {
                maxArea = currentArea
            }
        }
    }
    
    return maxArea
}
```

#### Complexity Analysis
- Time Complexity: O(n^2) because of the two nested loops.
- Space Complexity: O(1) since no additional space is used beyond variables.

---

### Divide and Conquer Approach

#### Intuition

This approach involves recursively finding the smallest bar in a given range of the histogram. The smallest bar determines a potential rectangle from the first to the last bar in that range. The solution then divides the problem into two subproblems to the left and the right of this smallest bar, recursively solving for the largest rectangle possible in those subranges. This strategy is much like the quicksort algorithm in its division of the problem space.

#### Go Code
```go
func largestRectangleAreaDivideAndConquer(heights []int) int {
    return calculateArea(heights, 0, len(heights)-1)
}

func calculateArea(heights []int, start, end int) int {
    if start > end {
        return 0
    }
    
    // Find the minimum height in the range
    minIndex := start
    for i := start + 1; i <= end; i++ {
        if heights[i] < heights[minIndex] {
            minIndex = i
        }
    }
    
    // Calculate the area with the minimum height bar
    maxAreaWithMinHeight := heights[minIndex] * (end - start + 1)
    
    // Recursively calculate the largest area on the left and right of the minimum height bar
    maxAreaLeft := calculateArea(heights, start, minIndex-1)
    maxAreaRight := calculateArea(heights, minIndex+1, end)
    
    // Return the maximum of the three possible areas
    return max(maxAreaWithMinHeight, maxAreaLeft, maxAreaRight)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

#### Complexity Analysis
- Time Complexity: O(n log n) on average, O(n^2) in the worst case when the minimal element is always at the beginning or the end.
- Space Complexity: O(n) due to the recursion stack used by divide and conquer.

---

### Stack-Based Optimal Approach

#### Intuition

For an optimal and very efficient solution, we use a stack. The stack helps track indices of bars in the histogram, where each bar is as tall or taller than the one before it. The trick here is to keep elements in the stack in non-decreasing order, so when a new, shorter bar is encountered, bars are popped from the stack one by one, calculating area as if they were the shortest bars in a rectangle. This approach allows us to find the largest possible rectangle efficiently by leveraging the stack's ordered nature.

#### Go Code
```go
func largestRectangleArea(heights []int) int {
    stack := []int{}
    maxArea := 0
    n := len(heights)
    
    for i := 0; i <= n; i++ {
        // Use 0 as a sentinel value to force emptying the stack at the end
        height := 0
        if i < n {
            height = heights[i]
        }
        
        // Ensure that stack maintains increasing order of heights
        for len(stack) != 0 && heights[stack[len(stack)-1]] > height {
            topIndex := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            
            // Determine the height and width of the potential largest area
            h := heights[topIndex]
            width := i
            if len(stack) != 0 {
                width = i - stack[len(stack)-1] - 1
            }
            maxArea = max(maxArea, h * width)
        }
        
        stack = append(stack, i)
    }
    
    return maxArea
}
```

#### Complexity Analysis
- Time Complexity: O(n) because each bar is pushed and popped from the stack at most once.
- Space Complexity: O(n) for the stack used to store the indices.

