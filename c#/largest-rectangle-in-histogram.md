# [Leetcode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Optimal Approach using Stack](#optimal-approach-using-stack)

### Brute Force Approach

#### Intuition:
The brute force approach involves calculating the maximum area of a rectangle for every possible pair of heights in the histogram. For each pair of heights, calculate the minimum height and multiply it by the width to find the area.

#### Steps:
1. Iterate over each bar of the histogram as the starting point.
2. For each starting point, extend the rectangle to the end of the histogram.
3. For each extension, calculate the minimum height so far and the corresponding area.
4. Keep track of the maximum area found.

#### Code:

```csharp
public class Solution {
    public int LargestRectangleArea(int[] heights) {
        int maxArea = 0;
        int n = heights.Length;
        
        for (int i = 0; i < n; i++) {
            int minHeight = heights[i];
            for (int j = i; j < n; j++) {
                minHeight = Math.Min(minHeight, heights[j]); // Find the minimum height from i to j
                int width = j - i + 1;                       // Calculate the width of the rectangle
                int area = minHeight * width;                // Calculate the area for this width and minHeight
                maxArea = Math.Max(maxArea, area);           // Update maxArea if we found a larger area
            }
        }
        
        return maxArea;
    }
}
```

#### Time Complexity:
- O(N^2): For each bar, we extend to the end of the histogram.
- N is the number of bars in the histogram.

#### Space Complexity:
- O(1): We only use a fixed amount of additional space.

---

### Optimal Approach using Stack

#### Intuition:
The stack-based approach efficiently finds and processes each possible rectangle in one pass by using a stack to keep track of the histogram's indices. As we iterate through the histogram, if a bar is shorter than the bar at the index stored on the stack, this indicates a boundary where we can no longer extend a rectangle. At that point, we resolve all rectangles involving the height at that index.

#### Steps:
1. Create a stack to keep track of indices.
2. Iterate through the bars of the histogram.
3. Whenever the current bar is shorter than the last bar on the stack, pop from the stack and calculate the area using the popped height as the shortest bar.
4. Continue this until the current bar is no longer shorter than the bar on top of the stack.
5. Ensure to append a 0-height bar at the end to flush all the heights in the stack.

#### Code:

```csharp
public class Solution {
    public int LargestRectangleArea(int[] heights) {
        Stack<int> stack = new Stack<int>();
        int maxArea = 0;
        int n = heights.Length;
        
        for (int i = 0; i <= n; i++) {
            int currentHeight = (i == n ? 0 : heights[i]);
            while (stack.Count > 0 && heights[stack.Peek()] > currentHeight) {
                int height = heights[stack.Pop()]; // Get the height of the bar that forms the largest rectangle
                int width = stack.Count == 0 ? i : i - stack.Peek() - 1; // Calculate the width
                maxArea = Math.Max(maxArea, height * width); // Update maxArea if necessary
            }
            stack.Push(i); // Push the current index to the stack
        }
        
        return maxArea;
    }
}
```

#### Time Complexity:
- O(N): Each index is pushed to and popped from the stack at most once.

#### Space Complexity:
- O(N): Stack space to store the indices of bars.

This stack-based solution is more efficient and is preferred for handling large datasets in this problem.

