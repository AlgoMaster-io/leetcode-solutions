# 4. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approach 1: Brute Force (Merge and Find Median)

### Solution
```java
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int[] merged = new int[m + n];

        // Merge the two arrays into one sorted array
        int i = 0, j = 0, k = 0;
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
        int totalLength = m + n;
        if (totalLength % 2 == 0) {
            return (merged[totalLength / 2 - 1] + merged[totalLength / 2]) / 2.0;
        } else {
            return merged[totalLength / 2];
        }
    }
}
```

## Approach 2: Binary Search on Partition Index (Optimal Solution)

### Solution
```java
// Time Complexity: O(log(min(m, n)))
// Space Complexity: O(1)
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            // Ensure nums1 is the smaller array
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length, n = nums2.length;
        int start = 0, end = m;

        while (start <= end) {
            int partitionX = start + (end - start) / 2;
            int partitionY = (m + n + 1) / 2 - partitionX;

            // Edge cases: Use Integer.MIN_VALUE and Integer.MAX_VALUE for out-of-bound partitions
            int maxX = (partitionX == 0) ? Integer.MIN_VALUE : nums1[partitionX - 1];
            int minX = (partitionX == m) ? Integer.MAX_VALUE : nums1[partitionX];
            int maxY = (partitionY == 0) ? Integer.MIN_VALUE : nums2[partitionY - 1];
            int minY = (partitionY == n) ? Integer.MAX_VALUE : nums2[partitionY];

            if (maxX <= minY && maxY <= minX) {
                // Correct partition found
                if ((m + n) % 2 == 0) {
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

        throw new IllegalArgumentException("Input arrays are not sorted or invalid.");
    }
}
```