# [Container With Most Water - LeetCode](https://leetcode.com/problems/container-with-most-water/)

## Approaches:
1. [Brute Force Approach](#brute-force-approach)
2. [Two Pointer Approach](#two-pointer-approach)

### Brute Force Approach

**Intuition:**

The naive approach is to check the area formed between every pair of lines and find the maximum area. For each pair of lines, calculate the area by taking the vertical line formed by the smaller height and multiply it with the horizontal distance between these lines.

**Algorithm:**
- Use a nested loop to consider every pair of lines `(i, j)` where `i < j`.
- For each pair, calculate the area as the minimum of the two heights times the distance between them.
- Keep track of the maximum area encountered.

**Time Complexity:** O(n^2)  
**Space Complexity:** O(1)

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int max_area = 0;
        int n = height.size();

        // Iterate over each pair (i, j)
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                // Calculate the height as the minimum of height[i] and height[j]
                int h = min(height[i], height[j]);
                // Calculate the width as the distance between the two lines
                int w = j - i;
                // Calculate the area
                int area = h * w;
                // Update the maximum area if the current area is larger
                max_area = max(max_area, area);
            }
        }
        
        return max_area;
    }
};
```

### Two Pointer Approach

**Intuition:**

To improve the algorithm, we can use the two-pointer technique. The idea is to start with the widest possible container and move the pointers inwards to find if a larger area can be formed. By moving the pointer corresponding to the shorter line inward, we might find a longer line that can form a container with a larger area.

**Algorithm:**
- Initialize two pointers, `left` at the start and `right` at the end of the array.
- While `left` < `right`:
  - Calculate the area with `heights[left]` and `heights[right]` as the height and `right - left` as the width.
  - Record this as the max area if it is larger than the current max area.
  - Move the pointer pointing to the shorter line inward by one position.
- Return the maximum area found.

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0; // Pointer at the start of the array
        int right = height.size() - 1; // Pointer at the end of the array
        int max_area = 0; // Initialize max_area to track the largest area found

        // While left pointer is less than right pointer
        while (left < right) {
            // Calculate the height as the minimum of height[left] and height[right]
            int current_height = min(height[left], height[right]);
            // Calculate the width as the distance between the two pointers
            int width = right - left;
            // Calculate the current area
            int current_area = current_height * width;
            // Update the maximum area if the current area is larger
            max_area = max(max_area, current_area);

            // Move the pointer of the shorter line inward
            if (height[left] < height[right]) {
                left++; // Move the left pointer rightwards
            } else {
                right--; // Move the right pointer leftwards
            }
        }

        return max_area;
    }
};
```

This two-pointer technique optimally finds the maximum container area by intelligently pruning the search space. By always moving the shorter pointer, it's guaranteed that any missed pair would not contain a greater area.

