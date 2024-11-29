# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        int totalWater = 0;

        // Iterate over each bar
        for (int i = 1; i < n - 1; i++) {
            int leftMax = 0;
            int rightMax = 0;

            // Find the maximum height to the left of the current bar
            for (int j = 0; j <= i; j++) {
                leftMax = max(leftMax, height[j]);
            }

            // Find the maximum height to the right of the current bar
            for (int j = i; j < n; j++) {
                rightMax = max(rightMax, height[j]);
            }

            // Calculate water trapped at the current bar
            totalWater += min(leftMax, rightMax) - height[i];
        }

        return totalWater;
    }
};
```

## Approach 2: Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size();
        if (n == 0) return 0;

        vector<int> leftMax(n);
        vector<int> rightMax(n);
        int totalWater = 0;

        // Compute leftMax array
        leftMax[0] = height[0];
        for (int i = 1; i < n; i++) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }

        // Compute rightMax array
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }

        // Calculate water trapped at each bar
        for (int i = 0; i < n; i++) {
            totalWater += min(leftMax[i], rightMax[i]) - height[i];
        }

        return totalWater;
    }
};
```

## Approach 3: Two Pointers (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int trap(vector<int>& height) {
        int left = 0, right = height.size() - 1;
        int leftMax = 0, rightMax = 0;
        int totalWater = 0;

        // Use two pointers to calculate trapped water
        while (left < right) {
            if (height[left] < height[right]) {
                // Update leftMax and calculate water at left pointer
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    totalWater += leftMax - height[left];
                }
                left++;
            } else {
                // Update rightMax and calculate water at right pointer
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    totalWater += rightMax - height[right];
                }
                right--;
            }
        }

        return totalWater;
    }
};
```

