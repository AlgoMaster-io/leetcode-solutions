# [Leetcode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approaches
- [Brute Force Approach](#brute-force-approach)
- [Stack-Based Approach](#stack-based-approach)

### Brute Force Approach

#### Intuition
The brute force approach involves iterating over every pair of bars as a possible base for a rectangle and calculating the area between them. By checking each potential rectangle, we ensure that we are considering all possible subarrays. 

For each bar, consider it as the left boundary of the rectangle, and iterate through the subsequent bars to find the minimum height of the rectangle while updating the maximum area.

#### Implementation

```python
def largestRectangleArea(heights):
    n = len(heights)
    max_area = 0
    
    # Try each bar as the starting point
    for i in range(n):
        min_height = heights[i]
        
        # Extend the rectangle to the right
        for j in range(i, n):
            min_height = min(min_height, heights[j])
            
            # Calculate area with (i, j) as boundaries
            max_area = max(max_area, min_height * (j - i + 1))
    
    return max_area
```

#### Time and Space Complexity
- **Time Complexity**: O(n^2), where n is the number of bars. We consider each pair of bars.
- **Space Complexity**: O(1), as no additional data structures are used besides a few variables.

### Stack-Based Approach

#### Intuition
The stack-based approach leverages a stack to efficiently find the nearest smaller bar to the left and right of each bar. We'll use the stack to keep track of indices of the bars in a way that helps easily calculate maximum rectangles. The idea is to iterate through each bar, push its index onto the stack as long as it is taller than the bar at the previous index. When we find a bar shorter than the one on top of the stack, we calculate the area with the bar at the current top of the stack as the smallest height since it could potentially extend the rectangle bounded by the previous smaller elements on the left and right.

#### Implementation

```python
def largestRectangleArea(heights):
    max_area = 0
    stack = []  # stack to store indices of the bars
    
    # Iterate over all bars
    for i in range(len(heights)):
        
        # If we encounter a bar that is smaller than the bar at the index
        # stored on top of the stack, we have to pop the stack and calculate area
        while stack and heights[stack[-1]] > heights[i]:
            # Pop the top element as it is no longer the smallest element
            height = heights[stack.pop()]
            
            # Calculate the width of the rectangle with that height
            # If stack is empty, it can extend back to start
            width = i if not stack else i - stack[-1] - 1
            
            # Calculate area and update max_area
            max_area = max(max_area, height * width)
        
        # Push the current index to the stack
        stack.append(i)
    
    # Clean up the remaining stack elements
    while stack:
        height = heights[stack.pop()]
        width = len(heights) if not stack else len(heights) - stack[-1] - 1
        max_area = max(max_area, height * width)

    return max_area
```

#### Time and Space Complexity
- **Time Complexity**: O(n), as each bar is pushed and popped from the stack at most once.
- **Space Complexity**: O(n), due to the use of the stack to store indices.

By using these methods, you can solve the "Largest Rectangle in Histogram" problem with varying degrees of efficiency, with the stack-based approach being the optimal solution in terms of time complexity.

