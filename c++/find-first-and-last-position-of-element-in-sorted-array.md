# [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## Approaches
- [Binary Search to find starting and ending position](#binary-search-to-find-starting-and-ending-position)

---

### Binary Search to Find Starting and Ending Position

**Intuition**:  
Since the array is sorted, binary search is an ideal approach to efficiently find the first and last positions of the target element. The binary search will be conducted in two separate passes:
1. First pass to find the first occurrence of the target.
2. Second pass to find the last occurrence of the target.

**Approach**:
- Conduct binary search twice:
  - First binary search to find the first position of the target:
    - Adjust `right` when the middle element is greater than or equal to the target.
    - Ensure that when the target is found, the search moves left (`right = mid - 1`) to validate if it's the first occurrence.
  - Second binary search to find the last position of the target:
    - Adjust `left` when the middle element is less than or equal to the target.
    - Ensure that when the target is found, the search moves right (`left = mid + 1`) to check if it's the last occurrence.

**Time Complexity**: O(log n), where n is the number of elements in the array. This is due to performing binary search twice.  
**Space Complexity**: O(1), only a fixed amount of space is used.

```cpp
#include <vector>
using namespace std;

class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        return {findFirstPosition(nums, target), findLastPosition(nums, target)};
    }
    
    // Helper function to find the first position of target
    int findFirstPosition(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        int firstPosition = -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            // If target is found, tentatively set firstPosition
            if (nums[mid] == target) {
                firstPosition = mid;
                // Move to the left part to check if there's an earlier occurrence
                right = mid - 1; 
            } else if (nums[mid] < target) {
                // Move right; target is greater
                left = mid + 1;
            } else {
                // Move left; target is smaller
                right = mid - 1;
            }
        }
        
        return firstPosition;
    }
    
    // Helper function to find the last position of target
    int findLastPosition(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        int lastPosition = -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            // If target is found, tentatively set lastPosition
            if (nums[mid] == target) {
                lastPosition = mid;
                // Move to the right part to check if there's a later occurrence
                left = mid + 1;
            } else if (nums[mid] < target) {
                // Move right; target is greater
                left = mid + 1;
            } else {
                // Move left; target is smaller
                right = mid - 1;
            }
        }
        
        return lastPosition;
    }
};
```

In this approach, the strategy is to make careful binary decisions to pinpoint the edge cases of a range within a sorted list. The goal is to leverage the properties of a binary search to deduce both the start and end boundaries for the appearances of the target value in the array.

