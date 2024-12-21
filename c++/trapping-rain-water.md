# [Leetcode 42: Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

### Approaches

- [Brute Force](#brute-force)
- [Optimized with Dynamic Programming Arrays](#optimized-with-dynamic-programming-arrays)
- [Two-Pointer Technique](#two-pointer-technique)

## Brute Force

### Intuition

The brute-force approach involves checking for each element how much water it can trap by evaluating the tallest bar on its left and the tallest bar on its right. The amount of water trapped on that specific bar will be the minimum of these two maximums minus the bar's height itself.

### Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty()) return 0;
        int n = height.size();
        int waterTrapped = 0;
        
        // Iterate through each bar
        for (int i = 0; i < n; ++i) {
            int maxLeft = 0, maxRight = 0;
            
            // Find the tallest bar to the left of the current bar
            for (int j = 0; j <= i; ++j) {
                maxLeft = max(maxLeft, height[j]);
            }
            
            // Find the tallest bar to the right of the current bar
            for (int j = i; j < n; ++j) {
                maxRight = max(maxRight, height[j]);
            }
            
            // Water trapped on the current bar
            waterTrapped += min(maxLeft, maxRight) - height[i];
        }
        
        return waterTrapped;
    }
};
```

### Time Complexity

- **Time Complexity**: O(n^2), where n is the number of bars, because for each bar, it scans all elements to its left and all elements to its right.
  
- **Space Complexity**: O(1), no additional space is used besides variables.

## Optimized with Dynamic Programming Arrays

### Intuition

We can improve the brute-force approach by precomputing the maximum height to the left and right of each bar using two arrays. This avoids the need for repeated maximum height calculations.

### Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty()) return 0;
        int n = height.size();
        int waterTrapped = 0;
        
        // Arrays to store the maximum height to the left and right of each bar
        vector<int> leftMax(n), rightMax(n);
        
        // Fill the leftMax array
        leftMax[0] = height[0];
        for (int i = 1; i < n; ++i) {
            leftMax[i] = max(leftMax[i - 1], height[i]);
        }
        
        // Fill the rightMax array
        rightMax[n - 1] = height[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            rightMax[i] = max(rightMax[i + 1], height[i]);
        }
        
        // Calculate trapped water using precomputed arrays
        for (int i = 0; i < n; ++i) {
            waterTrapped += min(leftMax[i], rightMax[i]) - height[i];
        }
        
        return waterTrapped;
    }
};
```

### Time Complexity

- **Time Complexity**: O(n), where n is the number of bars, as each of the three steps (filling `leftMax`, filling `rightMax`, and calculating the trapped water) is linear.
  
- **Space Complexity**: O(n), due to the use of two additional arrays to store left and right maximum heights.

## Two-Pointer Technique

### Intuition

We can further improve the space complexity by using two pointers to traverse the height array from both ends. By maintaining two pointers (`left` and `right`), we can decide which pointer to move based on which side has a smaller height. This technique efficiently tracks the left and right maximum heights using only variables.

### Code

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        if (height.empty()) return 0;
        int n = height.size();
        int left = 0, right = n - 1;
        int leftMax = 0, rightMax = 0;
        int waterTrapped = 0;
        
        // Two-pointer traversal
        while (left < right) {
            if (height[left] < height[right]) {
                // Process the left side
                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    waterTrapped += leftMax - height[left];
                }
                ++left;
            } else {
                // Process the right side
                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    waterTrapped += rightMax - height[right];
                }
                --right;
            }
        }
        
        return waterTrapped;
    }
};
```

### Time Complexity

- **Time Complexity**: O(n), where n is the number of bars, due to the single linear traversal of the array.
  
- **Space Complexity**: O(1), as only constant extra space is used for variables.

