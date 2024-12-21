# [Leetcode 35: Search Insert Position](https://leetcode.com/problems/search-insert-position/)

## Approaches

1. [Linear Search](#linear-search)
2. [Binary Search](#binary-search)

---

## Linear Search

### Intuition

The linear search approach involves iterating through the array to find the position where the target should be inserted. As we scan through the array, if we find a number that is greater than or equal to the target, that index can be returned as the insertion position. This is the simplest approach but not the most optimal one.

### Implementation

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        // Iterate through the array to find the appropriate insertion point
        for (int i = 0; i < nums.size(); ++i) {
            // If we find an element greater than or equal to the target,
            // target should be inserted at this position
            if (nums[i] >= target) {
                return i;
            }
        }
        // If the target is greater than all elements in the array,
        // it should be inserted at the end
        return nums.size();
    }
};
```

### Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of elements in the array. We might need to scan all elements in the worst case.
- **Space Complexity**: O(1), as the solution uses a constant amount of extra space.

---

## Binary Search

### Intuition

Binary search is a more efficient approach, especially for sorted arrays. Since the array is sorted, we can significantly reduce our search space by dividing it in half each time. The algorithm works by checking the middle element and narrowing down the search interval based on whether the target is less than, equal to, or greater than the middle element.

### Implementation

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        
        while (left <= right) {
            // Find the middle index, avoiding potential overflow
            int mid = left + (right - left) / 2;
            
            // Check if the middle element is the target
            if (nums[mid] == target) {
                return mid;
            }
            // If target is less than the middle element,
            // it must be in the left subarray
            else if (nums[mid] > target) {
                right = mid - 1;
            }
            // If target is greater than the middle element,
            // it must be in the right subarray
            else {
                left = mid + 1;
            }
        }
        
        // Left now points to the position where the target should be inserted
        return left;
    }
};
```

### Time and Space Complexity

- **Time Complexity**: O(log n), where n is the number of elements in the array. Each step reduces the search space by half.
- **Space Complexity**: O(1), as we're using only a constant amount of extra space.

Binary search is preferred for this problem due to its efficiency in handling larger datasets.

