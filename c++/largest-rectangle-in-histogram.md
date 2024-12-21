# [Leetcode 84: Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Better Optimal Approach - Divide and Conquer](#better-optimal-approach---divide-and-conquer)
- [Most Optimal Approach - Stack](#most-optimal-approach---stack)

### Brute Force Approach

**Intuition:**
The simplest way to solve this problem is to calculate the area for each possible rectangle that can be formed with each histogram bar as the shortest one. Iterate over each pair of bars and compute the maximum rectangle area possible including all bars between these two.

**Approach:**
1. Iterate over each bar as the starting point.
2. For each starting bar, iterate over every possible ending bar.
3. Calculate the area for all the bars between the starting and ending points using the minimum height.
4. Update the maximum area if the calculated area is larger.

**Complexity Analysis:**
- **Time Complexity:** O(n^2), where n is the number of bars in the histogram.
- **Space Complexity:** O(1), no additional space is used.

```cpp
int largestRectangleAreaBruteForce(vector<int>& heights) {
    int maxArea = 0;
    int n = heights.size();
    for (int i = 0; i < n; i++) {
        int minHeight = heights[i];
        for (int j = i; j < n; j++) {
            minHeight = min(minHeight, heights[j]); // Find the smallest bar in the current range
            maxArea = max(maxArea, minHeight * (j - i + 1)); // Calculate the area and update maxArea
        }
    }
    return maxArea;
}
```

### Better Optimal Approach - Divide and Conquer

**Intuition:**
This approach uses the idea of divide and conquer similar to QuickSort. The strategy is to find the minimum bar in the current range, calculate the maximum area using this bar, and then recursively calculate the maximum area for the left and right parts.

**Approach:**
1. Find the index of the minimum bar within the range.
2. Calculate the maximum rectangle area using this minimum bar as the smallest bar.
3. Recursively calculate the maximum area in the parts left and right of the minimum bar.

**Complexity Analysis:**
- **Time Complexity:** O(n log n) in the average case and O(n^2) in the worst case when the array is already sorted.
- **Space Complexity:** O(log n) for recursive stack space.

```cpp
int calculateArea(vector<int>& heights, int start, int end) {
    if (start > end) return 0;
    
    int minIndex = start;
    for (int i = start; i <= end; i++) {
        if (heights[i] < heights[minIndex]) {
            minIndex = i;
        }
    }
    
    // Calculate area with heights[minIndex] as the shortest height
    int areaUsingMin = heights[minIndex] * (end - start + 1);
    // Calculate area in the left and right parts
    int areaLeft = calculateArea(heights, start, minIndex - 1);
    int areaRight = calculateArea(heights, minIndex + 1, end);
    
    // Return the maximum area from three calculated areas
    return max(areaUsingMin, max(areaLeft, areaRight));
}

int largestRectangleAreaDivideAndConquer(vector<int>& heights) {
    return calculateArea(heights, 0, heights.size() - 1);
}
```

### Most Optimal Approach - Stack

**Intuition:**
Using a stack can help efficiently identify the largest rectangle. This approach involves maintaining a stack of indices in the histogram and ensures each bar is processed in an amortized constant time.

**Approach:**
1. Use a stack to keep track of the indices of the bars.
2. Iterate over each bar and maintain the stack such that the bars in stack are in increasing order of height.
3. When a bar is found smaller than the bar at the top of the stack, this bar becomes the right boundary of the rectangle with height of the stack's top index.
4. Pop the top of the stack and calculate the area with this height as the smallest bar.
5. Continue this process until the stack is empty or the current bar is greater.
6. Finally, process any remaining bars in the stack after iteration ends.

**Complexity Analysis:**
- **Time Complexity:** O(n), where n is the number of bars in the histogram.
- **Space Complexity:** O(n), used by the stack.

```cpp
int largestRectangleAreaStack(vector<int>& heights) {
    stack<int> st;
    int maxArea = 0;
    int n = heights.size();

    for (int i = 0; i <= n; i++) {
        // Use a sentinel value to empty the stack at the end
        while (!st.empty() && (i == n || heights[st.top()] > heights[i])) {
            int height = heights[st.top()];
            st.pop();
            int width = st.empty() ? i : i - st.top() - 1; // Calculate the width using the remaining indices in the stack
            maxArea = max(maxArea, height * width); // Update maxArea
        }
        st.push(i);
    }

    return maxArea;
}
```

