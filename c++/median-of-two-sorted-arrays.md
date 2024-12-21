# [Median of Two Sorted Arrays - LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approaches
1. [Basic Approach: Merge and Find](#basic-approach)
2. [Optimized Method: Binary Search](#optimized-method)

---

## Basic Approach: Merge and Find

### Intuition
The most straightforward approach to find the median of two sorted arrays is to first merge both arrays into a single sorted array and then find the median of the resulting array. While this approach is simple to implement, it does not take advantage of the fact that both input arrays are already sorted.

### Implementation

```cpp
#include <vector>
#include <algorithm>

double findMedianSortedArrays(std::vector<int>& nums1, std::vector<int>& nums2) {
    // Step 1: Initialize a result array to store the merged elements
    std::vector<int> merged;
    merged.reserve(nums1.size() + nums2.size());

    // Step 2: Using iterators to traverse both arrays
    auto it1 = nums1.begin();
    auto it2 = nums2.begin();

    // Step 3: Merging both arrays into one
    while (it1 != nums1.end() && it2 != nums2.end()) {
        if (*it1 < *it2) {
            merged.push_back(*it1);
            ++it1;
        } else {
            merged.push_back(*it2);
            ++it2;
        }
    }

    // Step 4: Append any remaining elements from nums1 or nums2
    while (it1 != nums1.end()) {
        merged.push_back(*it1);
        ++it1;
    }
    
    while (it2 != nums2.end()) {
        merged.push_back(*it2);
        ++it2;
    }

    // Step 5: Calculate the median from the merged array
    int n = merged.size();
    if (n % 2 == 1) {
        return merged[n / 2];
    } else {
        return (merged[(n - 1) / 2] + merged[n / 2]) / 2.0;
    }
}
```

### Time Complexity
- **O(n + m)** where `n` and `m` are the lengths of the two arrays. This is due to the full traversal of both arrays.

### Space Complexity
- **O(n + m)** for storing the merged array.

---

## Optimized Method: Binary Search

### Intuition
To optimize the process, one can take advantage of the sorted properties of both arrays and use a binary search approach. The main idea is to partition both arrays into two halves such that the maximum element on the left is less than the minimum element on the right. This is a more efficient method, reducing complexity by not requiring additional space allocation for merge operations.

### Implementation

```cpp
#include <vector>
#include <algorithm>
#include <climits>

double findMedianSortedArrays(std::vector<int>& nums1, std::vector<int>& nums2) {
    // Ensure nums1 is the smaller array for reduced time complexity
    if (nums1.size() > nums2.size()) {
        return findMedianSortedArrays(nums2, nums1);
    }

    int x = nums1.size();
    int y = nums2.size();
    int low = 0, high = x;

    while (low <= high) {
        // Step 1: Partition nums1 and nums2
        int partitionX = (low + high) / 2;
        int partitionY = (x + y + 1) / 2 - partitionX;

        // Step 2: Use infinity where partition is the end
        int maxX = (partitionX == 0) ? INT_MIN : nums1[partitionX - 1];
        int minX = (partitionX == x) ? INT_MAX : nums1[partitionX];

        int maxY = (partitionY == 0) ? INT_MIN : nums2[partitionY - 1];
        int minY = (partitionY == y) ? INT_MAX : nums2[partitionY];

        // Step 3: Check if we have found the correct partitions
        if (maxX <= minY && maxY <= minX) {
            // Step 4: Calculate median based on parity of total element count
            if ((x + y) % 2 == 0) {
                return (std::max(maxX, maxY) + std::min(minX, minY)) / 2.0;
            } else {
                return std::max(maxX, maxY);
            }
        }
        // Step 5: Binary search adjustment
        else if (maxX > minY) {
            high = partitionX - 1;
        } else {
            low = partitionX + 1;
        }
    }

    throw std::invalid_argument("Input arrays are not valid");
}
```

### Time Complexity
- **O(log(min(n, m)))** where `n` and `m` are the lengths of the two arrays. We perform binary search on the smaller array.

### Space Complexity
- **O(1)** as no additional space is required other than variables used for binary search partitions.

