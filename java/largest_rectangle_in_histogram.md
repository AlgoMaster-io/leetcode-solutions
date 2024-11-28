# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Brute Force

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int largestRectangleArea(int[] heights) {
        int maxArea = 0;

        // Check every pair of bars as the boundaries of the rectangle
        for (int i = 0; i < heights.length; i++) {
            int minHeight = heights[i];

            for (int j = i; j < heights.length; j++) {
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
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Stack;

public class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack<Integer> stack = new Stack<>(); // Stack stores indices
        int maxArea = 0;
        int n = heights.length;

        for (int i = 0; i <= n; i++) {
            // Use 0 height as a sentinel value for the end of the array
            int currentHeight = (i == n) ? 0 : heights[i];

            // Process and calculate area for indices in the stack
            while (!stack.isEmpty() && currentHeight < heights[stack.peek()]) {
                int height = heights[stack.pop()]; // Height of the rectangle
                int width = stack.isEmpty() ? i : i - stack.peek() - 1; // Width of the rectangle
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
```java
// Time Complexity: O(n log n) on average, O(n^2) in the worst case
// Space Complexity: O(n) due to recursion
public class Solution {
    public int largestRectangleArea(int[] heights) {
        return calculateArea(heights, 0, heights.length - 1);
    }

    private int calculateArea(int[] heights, int start, int end) {
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
        int leftArea = calculateArea(heights, start, minIndex - 1);
        int rightArea = calculateArea(heights, minIndex + 1, end);

        return Math.max(currentArea, Math.max(leftArea, rightArea));
    }
}
```