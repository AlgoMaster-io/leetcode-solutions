# [Leetcode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimal Approach using Stack](#optimal-approach-using-stack)

---

## Brute Force Approach

### Intuition

In the brute force approach, we attempt to calculate the area of the rectangle for every pair of bars in the histogram. For each bar, we attempt to expand the rectangle as much as possible under the constraint that it cannot include shorter bars than the current one. We calculate the maximum area for each rectangle starting at every bar, and find the overall maximum.

### Approach

1. Initialize a variable `max_area` to zero for storing the maximum area found.
2. Iterate over each bar `i` of the histogram.
3. For each bar `i`, determine the minimum height for rectangles starting with subsequent bars `j`.
4. Calculate the area for each rectangle and update `max_area` with the larger value found.
5. Return `max_area` after evaluating all possible rectangles.

### Code

```typescript
function largestRectangleArea(heights: number[]): number {
    let max_area = 0;
    const n = heights.length;
    
    // Loop over each bar
    for (let i = 0; i < n; i++) {
        // Minimum height for current starting index `i`
        let minHeight = heights[i];
        
        // Loop to calculate area with each subsequent bar
        for (let j = i; j < n; j++) {
            minHeight = Math.min(minHeight, heights[j]);
            const area = minHeight * (j - i + 1);
            max_area = Math.max(max_area, area);
        }
    }
    
    return max_area;
}
```

### Complexity

- **Time Complexity:** O(n^2). We run two nested loops over the bars.
- **Space Complexity:** O(1). Uses constant extra space.

---

## Optimal Approach using Stack

### Intuition

The optimal approach leverages a stack to efficiently find the largest rectangle area in `O(n)` time. The stack helps keep track of indices of the histogram bars such that the bars are in increasing height order. When we find a bar shorter than the bar at the stack's top, it determines that we've found the right boundary for the rectangle formed with the height of the popped bar. The left boundary is the index of the current top of the stack.

### Approach

1. Create a stack to store indices.
2. Initialize `max_area` to 0.
3. Iterate through the bars while considering a bar '0' at the end to flush out remaining indices in the stack.
4. If the stack is empty or current bar is higher than the height of the index in the stack top, push current index onto the stack.
5. If the current bar is lower than the top of the stack, pop the stack, calculate the area where:
   - The height is the height of the bar at popped index.
   - The width is the difference between the current index and the index at the new stack top minus one (or just the current index if stack is empty).
6. Update `max_area` with the computed area.
7. Return `max_area`.

### Code

```typescript
function largestRectangleArea(heights: number[]): number {
    const stack: number[] = [];
    let max_area = 0;
    const n = heights.length;

    // Handle all heights plus a sentinel value (0 height)
    for (let i = 0; i <= n; i++) {
        const currentHeight = (i === n) ? 0 : heights[i];
        
        // Keep stack in increasing height order
        while (stack.length !== 0 && heights[stack[stack.length - 1]] > currentHeight) {
            // The index of the bar for which we calculate area
            const heightIndex = stack.pop()!;
            // Determine the height
            const height = heights[heightIndex];
            // Determine the width
            const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
            // Calculate area
            const area = height * width;
            // Update max_area
            max_area = Math.max(max_area, area);
        }
        
        // Push the current bar index to stack
        stack.push(i);
    }

    return max_area;
}
```

### Complexity

- **Time Complexity:** O(n). Each bar is pushed and popped from the stack at most once.
- **Space Complexity:** O(n). Stack can contain all bars in a single increasing sequence.

