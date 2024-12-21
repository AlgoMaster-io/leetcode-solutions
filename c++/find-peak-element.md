# [Leetcode 162: Find Peak Element](https://leetcode.com/problems/find-peak-element/)

- [Approach 1: Linear Scan](#approach-1-linear-scan)
- [Approach 2: Binary Search](#approach-2-binary-search)

## Approach 1: Linear Scan

### Intuition
The simplest way to find a peak element is to perform a linear scan over the array. A peak element is an element that is greater than its neighbors. If you compare every element to its neighbors, you can directly return as soon as a peak is identified.

### Step-by-step reason:
1. Since the ends of the array can also be considered as peaks if they are greater than their respective one neighbor, we should check for that.
2. Iterate from left to right over the array.
3. Check if the current element is greater than its neighbors. If it is, then it is our peak element.

### Code
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        
        // Checking if the first element is a peak
        if (n == 1 || nums[0] > nums[1]) return 0;
        
        // Checking if the last element is a peak
        if (nums[n-1] > nums[n-2]) return n-1;
        
        // Checking peaks within the array bounds
        for (int i = 1; i < n - 1; ++i) {
            // If current is greater than both of its neighbors, it's a peak
            if (nums[i] > nums[i-1] && nums[i] > nums[i+1]) {
                return i;
            }
        }
        
        return -1; // This line generally never reaches as the problem assures a peak exists
    }
};
```

### Complexity Analysis
- **Time Complexity:** O(n) Since we might have to scan the entire array in the worst case.
- **Space Complexity:** O(1) No extra space is used.

## Approach 2: Binary Search

### Intuition
To achieve an optimal solution, consider using a binary search approach. The problem guarantees the existence of a peak element, allowing safe usage of binary search by comparing midpoints to adjacent elements and deciding the search direction based on these conditions. If the midpoint itself isn't a peak, moving towards the direction where the neighbor is greater ensures an upward slope exists, guiding us to a peak.

### Step-by-step reason:
1. Initialize two pointers, `left` and `right`, at the start and end of the array respectively.
2. Perform binary search:
   - Calculate the mid-point (`mid`) of the array.
   - Check if the mid-point is a peak:
     - It is a peak if `nums[mid] > nums[mid-1]` and `nums[mid] > nums[mid+1]`.
   - If not a peak:
     - If the element at `mid+1` is greater than `mid`, then a peak lies on the right.
     - Otherwise, a peak is on the left.
3. Adjust `left` or `right` accordingly to narrow down to a peak.

### Code
```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        
        while (left < right) {
            int mid = left + (right - left) / 2;

            // Check direction of the slope and move accordingly
            if (nums[mid] > nums[mid + 1]) {
                // If mid element is greater than its right neighbor, peak lies to the left
                right = mid;
            } else {
                // else the peak lies to the right
                left = mid + 1;
            }
        }

        // Left points to the peak
        return left;
    }
};
```

### Complexity Analysis
- **Time Complexity:** O(log n), thanks to the binary search reducing the search space by half each time.
- **Space Complexity:** O(1), as it requires no additional storage apart from input.

