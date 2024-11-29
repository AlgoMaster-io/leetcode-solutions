# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approach 1: Using Built-in Sorting (Simplest)

### Solution
```cpp
// Time Complexity: O((m + n) log(m + n))
// Space Complexity: O(1)
#include <algorithm>

class Solution {
public:
    void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n) {
        // Copy nums2 into nums1
        std::copy(nums2.begin(), nums2.begin() + n, nums1.begin() + m);

        // Sort the combined array
        std::sort(nums1.begin(), nums1.begin() + m + n);
    }
};
```

## Approach 2: Two Pointers with Temporary Array

### Solution
```cpp
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
class Solution {
public:
    void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n) {
        std::vector<int> temp(m + n);
        int p1 = 0, p2 = 0, p = 0;

        // Merge elements from nums1 and nums2 into temp
        while (p1 < m && p2 < n) {
            if (nums1[p1] <= nums2[p2]) {
                temp[p++] = nums1[p1++];
            } else {
                temp[p++] = nums2[p2++];
            }
        }

        // Copy remaining elements from nums1
        while (p1 < m) {
            temp[p++] = nums1[p1++];
        }

        // Copy remaining elements from nums2
        while (p2 < n) {
            temp[p++] = nums2[p2++];
        }

        // Copy temp back to nums1
        std::copy(temp.begin(), temp.begin() + m + n, nums1.begin());
    }
};
```

## Approach 3: Two Pointers from End (Optimal)

### Solution
```cpp
// Time Complexity: O(m + n)
// Space Complexity: O(1)
class Solution {
public:
    void merge(std::vector<int>& nums1, int m, std::vector<int>& nums2, int n) {
        int p1 = m - 1; // Pointer for the end of nums1's initial values
        int p2 = n - 1; // Pointer for the end of nums2
        int p = m + n - 1; // Pointer for the end of nums1's total capacity

        // Start merging from the back
        while (p1 >= 0 && p2 >= 0) {
            if (nums1[p1] > nums2[p2]) {
                nums1[p] = nums1[p1];
                p1--;
            } else {
                nums1[p] = nums2[p2];
                p2--;
            }
            p--;
        }

        // If there are leftover elements in nums2
        while (p2 >= 0) {
            nums1[p] = nums2[p2];
            p2--;
            p--;
        }
    }
};
```

