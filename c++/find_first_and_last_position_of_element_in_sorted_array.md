# 34. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approach 1: Linear Search

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
#include <vector>

class Solution {
public:
    std::vector<int> searchRange(std::vector<int>& nums, int target) {
        int first = -1, last = -1;

        // Traverse the array to find the first and last positions
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == target) {
                if (first == -1) {
                    first = i; // Mark the first occurrence
                }
                last = i; // Continuously update the last occurrence
            }
        }

        return {first, last};
    }
};
```

## Approach 2: Binary Search (Two Separate Searches)

### Solution
```cpp
// Time Complexity: O(log n)
// Space Complexity: O(1)
#include <vector>

class Solution {
public:
    std::vector<int> searchRange(std::vector<int>& nums, int target) {
        int first = findPosition(nums, target, true);  // Find first occurrence
        int last = findPosition(nums, target, false); // Find last occurrence
        return {first, last};
    }

private:
    int findPosition(const std::vector<int>& nums, int target, bool findFirst) {
        int start = 0, end = nums.size() - 1, position = -1;

        while (start <= end) {
            int mid = start + (end - start) / 2;

            if (nums[mid] == target) {
                position = mid; // Record the position
                if (findFirst) {
                    end = mid - 1; // Narrow search to the left
                } else {
                    start = mid + 1; // Narrow search to the right
                }
            } else if (nums[mid] < target) {
                start = mid + 1; // Search in the right half
            } else {
                end = mid - 1; // Search in the left half
            }
        }

        return position;
    }
};
```

## Approach 3: Optimized Binary Search (Combined Search for Both Positions)

### Solution
```cpp
// Time Complexity: O(log n)
// Space Complexity: O(1)
#include <vector>

class Solution {
public:
    std::vector<int> searchRange(std::vector<int>& nums, int target) {
        std::vector<int> result = {-1, -1};
        if (nums.empty()) {
            return result; // Return immediately if array is empty
        }

        // Find the first occurrence
        int start = 0, end = nums.size() - 1;
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] < target) {
                start = mid + 1; // Search in the right half
            } else {
                end = mid - 1; // Search in the left half
            }
            if (nums[mid] == target) {
                result[0] = mid; // Update first position
            }
        }

        // Reset variables to find the last occurrence
        start = 0;
        end = nums.size() - 1;

        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (nums[mid] <= target) {
                start = mid + 1; // Search in the right half
            } else {
                end = mid - 1; // Search in the left half
            }
            if (nums[mid] == target) {
                result[1] = mid; // Update last position
            }
        }

        return result;
    }
};
```
