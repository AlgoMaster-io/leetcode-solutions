# [Leetcode 33: Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## Approaches
- [Approach 1: Linear Search](#approach-1-linear-search)
- [Approach 2: Modified Binary Search](#approach-2-modified-binary-search)

---

## Approach 1: Linear Search
### Intuition
The simplest way to solve the problem is to perform a linear search over the array. Given that the array is not sorted in the conventional sense due to rotation, we can iterate through the array from the start to the end, comparing each element with the target.

### Code
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        // Iterate over each element in the array
        for (int i = 0; i < nums.size(); i++) {
            // Check if the current element is the target
            if (nums[i] == target) {
                return i; // Return the index if found
            }
        }
        return -1; // Return -1 if the target is not found
    }
};
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the number of elements in the array. In the worst case, we have to check each element.
- **Space Complexity**: O(1), no extra space is used aside from the input.

---

## Approach 2: Modified Binary Search
### Intuition
Since the array is rotated and still retains some characteristics of sorted subarrays, we can utilize a modified binary search. Here's the step-by-step intuition:
1. Identify the mid-point of the array.
2. Determine which half is sorted by comparing the mid-point value with the start and end of the current search space.
3. Check if the target falls within the sorted section:
   - If yes, adjust the search space to focus on this section.
   - If no, adjust the search space to the other section.
4. Continue the binary search until the target is found or the search space is exhausted.

### Code
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        
        // Perform binary search
        while (left <= right) {
            int mid = left + (right - left) / 2; // Calculate mid point safely
            
            // Check if the mid-point is the target
            if (nums[mid] == target) {
                return mid;
            }
            
            // Determine which side is sorted
            if (nums[left] <= nums[mid]) {
                // Left part is sorted
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1; // Search in the left part
                } else {
                    left = mid + 1; // Search in the right part
                }
            } else {
                // Right part is sorted
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1; // Search in the right part
                } else {
                    right = mid - 1; // Search in the left part
                }
            }
        }
        
        return -1; // Target is not found
    }
};
```

### Complexity Analysis
- **Time Complexity**: O(log n), leveraging binary search, which halves the search space in each step.
- **Space Complexity**: O(1), operates in constant space as no additional data structures are used.

