# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int maxArea = 0;

        // Check every pair of bars as the boundaries of the rectangle
        for (int i = 0; i < heights.size(); i++) {
            int minHeight = heights[i];

            for (int j = i; j < heights.size(); j++) {
                // Update the minimum height in the current range
                minHeight = min(minHeight, heights[j]);
                // Calculate the area and update maxArea if needed
                maxArea = max(maxArea, minHeight * (j - i + 1));
            }
        }

        return maxArea;
    }
};
```

## Approach 2: Using a Stack (Optimal Solution)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
#include <stack>
#include <vector>

using namespace std;

class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> stack; // Stack stores indices
        int maxArea = 0;
        int n = heights.size();

        for (int i = 0; i <= n; i++) {
            // Use 0 height as a sentinel value for the end of the array
            int currentHeight = (i == n) ? 0 : heights[i];

            // Process and calculate area for indices in the stack
            while (!stack.empty() && currentHeight < heights[stack.top()]) {
                int height = heights[stack.top()];
                stack.pop();
                int width = stack.empty() ? i : i - stack.top() - 1; // Width of the rectangle
                maxArea = max(maxArea, height * width); // Update maxArea
            }

            stack.push(i); // Push the current index
        }

        return maxArea;
    }
};
```

## Approach 3: Divide and Conquer

### Solution
```cpp
// Time Complexity: O(n log n) on average, O(n^2) in the worst case
// Space Complexity: O(n) due to recursion
#include <vector>

using namespace std;

class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        return calculateArea(heights, 0, heights.size() - 1);
    }

private:
    int calculateArea(vector<int>& heights, int start, int end) {
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

        return max(currentArea, max(leftArea, rightArea));
    }
};
```

