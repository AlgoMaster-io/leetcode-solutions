# 4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approach 1: Brute Force (Merge and Find Median)

### Solution
```javascript
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
class Solution {
    findMedianSortedArrays(nums1, nums2) {
        const m = nums1.length, n = nums2.length;
        const merged = new Array(m + n);

        // Merge the two arrays into one sorted array
        let i = 0, j = 0, k = 0;
        while (i < m && j < n) {
            if (nums1[i] < nums2[j]) {
                merged[k++] = nums1[i++];
            } else {
                merged[k++] = nums2[j++];
            }
        }

        // Add remaining elements from nums1
        while (i < m) {
            merged[k++] = nums1[i++];
        }

        // Add remaining elements from nums2
        while (j < n) {
            merged[k++] = nums2[j++];
        }

        // Find and return the median
        const totalLength = m + n;
        if (totalLength % 2 === 0) {
            return (merged[Math.floor(totalLength / 2) - 1] + merged[Math.floor(totalLength / 2)]) / 2.0;
        } else {
            return merged[Math.floor(totalLength / 2)];
        }
    }
}
```

## Approach 2: Binary Search on Partition Index (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(log(min(m, n)))
// Space Complexity: O(1)
class Solution {
    findMedianSortedArrays(nums1, nums2) {
        if (nums1.length > nums2.length) {
            // Ensure nums1 is the smaller array
            return this.findMedianSortedArrays(nums2, nums1);
        }

        const m = nums1.length, n = nums2.length;
        let start = 0, end = m;

        while (start <= end) {
            const partitionX = Math.floor((start + end) / 2);
            const partitionY = Math.floor((m + n + 1) / 2) - partitionX;

            // Edge cases: Use -Infinity and Infinity for out-of-bound partitions
            const maxX = (partitionX === 0) ? -Infinity : nums1[partitionX - 1];
            const minX = (partitionX === m) ? Infinity : nums1[partitionX];
            const maxY = (partitionY === 0) ? -Infinity : nums2[partitionY - 1];
            const minY = (partitionY === n) ? Infinity : nums2[partitionY];

            if (maxX <= minY && maxY <= minX) {
                // Correct partition found
                if ((m + n) % 2 === 0) {
                    // Even total length: Average of max of left and min of right
                    return (Math.max(maxX, maxY) + Math.min(minX, minY)) / 2.0;
                } else {
                    // Odd total length: Max of left partition
                    return Math.max(maxX, maxY);
                }
            } else if (maxX > minY) {
                // Move left in nums1
                end = partitionX - 1;
            } else {
                // Move right in nums1
                start = partitionX + 1;
            }
        }

        throw new Error("Input arrays are not sorted or invalid.");
    }
}
```

