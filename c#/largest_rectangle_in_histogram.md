# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int LargestRectangleArea(int[] heights) {
        int maxArea = 0;

        // Check every pair of bars as the boundaries of the rectangle
        for (int i = 0; i < heights.Length; i++) {
            int minHeight = heights[i];

            for (int j = i; j < heights.Length; j++) {
                // Update the minimum height in the current range
                minHeight = Math.Min(minHeight, heights[j]);
                // Calculate the area and update maxArea if needed
                maxArea = Math.Max(maxArea, minHeight * (j - i + 1));
            }
        }

        return maxArea;
    }
}
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int LargestRectangleArea(int[] heights) {
        Stack<int> stack = new Stack<int>(); // Stack stores indices
        int maxArea = 0;
        int n = heights.Length;

        for (int i = 0; i <= n; i++) {
            // Use 0 height as a sentinel value for the end of the array
            int currentHeight = (i == n) ? 0 : heights[i];

            // Process and calculate area for indices in the stack
            while (stack.Count > 0 && currentHeight < heights[stack.Peek()]) {
                int height = heights[stack.Pop()]; // Height of the rectangle
                int width = stack.Count == 0 ? i : i - stack.Peek() - 1; // Width of the rectangle
                maxArea = Math.Max(maxArea, height * width); // Update maxArea
            }

            stack.Push(i); // Push the current index
        }

        return maxArea;
    }
}
```

## Approach 3: Divide and Conquer

### Solution
csharp
```csharp
// Time Complexity: O(n log n) on average, O(n^2) in the worst case
// Space Complexity: O(n) due to recursion
public class Solution {
    public int LargestRectangleArea(int[] heights) {
        return CalculateArea(heights, 0, heights.Length - 1);
    }

    private int CalculateArea(int[] heights, int start, int end) {
        if (start > end) return 0;

        // Find the index of the minimum height in the range
        int minIndex = start;
        for (int i = start; i <= end; i++) {
            if (heights[i] < heights[minIndex]) {
                minIndex = i;
            }
        }

        // Calculate the maximum area by dividing the problem
        int currentArea = heights[minIndex] * (end - start + 1);
        int leftArea = CalculateArea(heights, start, minIndex - 1);
        int rightArea = CalculateArea(heights, minIndex + 1, end);

        return Math.Max(currentArea, Math.Max(leftArea, rightArea));
    }
}
```

