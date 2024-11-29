# [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Approach 1: Brute Force

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int maxArea(vector<int>& height) {
        int maxArea = 0;

        // Check all pairs of lines
        for (int i = 0; i < height.size(); i++) {
            for (int j = i + 1; j < height.size(); j++) {
                // Calculate the area for the current pair of lines
                int area = min(height[i], height[j]) * (j - i);
                maxArea = max(maxArea, area); // Update max area if needed
            }
        }

        return maxArea;
    }
};
```

## Approach 2: Two Pointers (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0; // Start pointer
        int right = height.size() - 1; // End pointer
        int maxArea = 0; // Initialize max area

        // Iterate until the two pointers meet
        while (left < right) {
            // Calculate the current area
            int area = min(height[left], height[right]) * (right - left);
            maxArea = max(maxArea, area); // Update max area if needed

            // Move the pointer pointing to the shorter line
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
};
```
