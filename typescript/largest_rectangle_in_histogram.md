# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
    largestRectangleArea(heights: number[]): number {
        let maxArea = 0;

        // Check every pair of bars as the boundaries of the rectangle
        for (let i = 0; i < heights.length; i++) {
            let minHeight = heights[i];

            for (let j = i; j < heights.length; j++) {
                // Update the minimum height in the current range
                minHeight = Math.min(minHeight, heights[j]);
                // Calculate the area and update maxArea if needed
                maxArea = Math.max(maxArea, minHeight * (j - i + 1));
            }
        }

        return maxArea;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    largestRectangleArea(heights: number[]): number {
        const stack: number[] = []; // Stack stores indices
        let maxArea = 0;
        const n = heights.length;

        for (let i = 0; i <= n; i++) {
            // Use 0 height as a sentinel value for the end of the array
            const currentHeight = (i === n) ? 0 : heights[i];

            // Process and calculate area for indices in the stack
            while (stack.length > 0 && currentHeight < heights[stack[stack.length - 1]]) {
                const height = heights[stack.pop()!]; // Height of the rectangle
                const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1; // Width of the rectangle
                maxArea = Math.max(maxArea, height * width); // Update maxArea
            }

            stack.push(i); // Push the current index
        }

        return maxArea;
    }
}
```

## Approach 3: Divide and Conquer

### Solution
typescript
```typescript
// Time Complexity: O(n log n) on average, O(n^2) in the worst case
// Space Complexity: O(n) due to recursion
class Solution {
    largestRectangleArea(heights: number[]): number {
        return this.calculateArea(heights, 0, heights.length - 1);
    }

    private calculateArea(heights: number[], start: number, end: number): number {
        if (start > end) return 0;

        // Find the index of the minimum height in the range
        let minIndex = start;
        for (let i = start; i <= end; i++) {
            if (heights[i] < heights[minIndex]) {
                minIndex = i;
            }
        }

        // Calculate the maximum area by dividing the problem
        const currentArea = heights[minIndex] * (end - start + 1);
        const leftArea = this.calculateArea(heights, start, minIndex - 1);
        const rightArea = this.calculateArea(heights, minIndex + 1, end);

        return Math.max(currentArea, Math.max(leftArea, rightArea));
    }
}
```

