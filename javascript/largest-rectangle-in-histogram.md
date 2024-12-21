# [Leetcode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Navigation

- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach using a Stack](#optimized-approach-using-a-stack)

### Brute Force Approach

**Intuition:**

The brute force approach involves considering every possible rectangle that can be formed between two bars. For each pair of bars, calculate the minimum height within these bars and then compute the area. This approach ensures that we look at all potential rectangles, but it will be inefficient due to a double nested loop structure.

**Steps:**
1. Iterate over each bar as a potential starting point.
2. For every starting point, extend the rectangle to the right, one bar at a time.
3. Compute the minimum height encountered so far.
4. Calculate the area using `minimum height * width`.
5. Keep track of the maximum area calculated.

**Time Complexity:** O(n^2)
  
**Space Complexity:** O(1)

```javascript
function largestRectangleArea(heights) {
    let maxArea = 0;
    const n = heights.length;
    
    // Iterate over each bar as a starting point
    for (let i = 0; i < n; i++) {
        let minHeight = heights[i];
        
        // Extend to the right from the starting point
        for (let j = i; j < n; j++) {
            // Update the minimum height within the current window
            minHeight = Math.min(minHeight, heights[j]);
            
            // Calculate the area for the current window
            const width = j - i + 1;
            const area = minHeight * width;
            
            // Track the maximum area found
            maxArea = Math.max(maxArea, area);
        }
    }
    
    return maxArea;
}
```

### Optimized Approach using a Stack

**Intuition:**

This approach utilizes a stack to maintain indices of the histogram bars in increasing order of their heights. The stack helps track the start of the potential widest rectangle for each bar. When a bar is encountered that is shorter than the bar at the top of the stack, compute the area with the bar at the stack's top as the smallest (limiting) height.

**Steps:**
1. Initialize an empty stack and a variable for maximum area.
2. Iterate over the bars. For each bar:
   - If the current bar is greater than the bar at the stack's top, push it onto the stack.
   - Else, pop bars from the stack until the current bar is taller than the bar at the stack's top.
   - For each pop, calculate the potential rectangle area using the popped bar as the shortest height.
   - Continue iterating until all bars are processed.
3. If the stack is not empty, compute the area for the increasing sequence of bars remaining in the stack.

**Time Complexity:** O(n) - Each bar is pushed and popped from the stack once.

**Space Complexity:** O(n) - The stack can contain up to `n` elements in the worst case.

```javascript
function largestRectangleArea(heights) {
    let stack = []; // Stack to store indices of bars
    let maxArea = 0;
    const n = heights.length;
    
    for (let i = 0; i <= n; i++) {
        // Use a dummy `0` height for completing any left bars in stack
        const currentHeight = (i === n) ? 0 : heights[i];
        
        while (stack.length && currentHeight < heights[stack[stack.length - 1]]) {
            // Pop out the height of the bar from the top of the stack
            const height = heights[stack.pop()];
            // Determine the width for the current popped bar
            const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
            // Calculate area with the popped bar as smallest height
            const area = height * width;
            maxArea = Math.max(maxArea, area);
        }
        
        // Push the current index to the stack
        stack.push(i);
    }
    
    return maxArea;
}
```

This approach effectively reduces the time complexity by leveraging a stack to systematically and efficiently calculate rectangular areas as we traverse the histogram.

