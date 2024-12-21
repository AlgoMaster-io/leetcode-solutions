# [Leetcode 4: Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approach:
- [Brute Force](#brute-force)
- [Binary Search (Optimal)](#binary-search-optimal)

---

## Brute Force

### Intuition:

The simplest way to find the median of two sorted arrays is to merge the two arrays into one sorted array, then find the median of the merged array. Although this approach straightforwardly solves the problem, it is not efficient enough for large inputs due to unnecessary extra space usage and time complexity.

### Steps:
1. Merge the two sorted arrays into a single array.
2. Sort the merged array.
3. Find the median of the merged array. If the length is odd, the median is the middle element. If the length is even, it is the average of the two middle elements.

### Code:

```csharp
public class Solution {
    public double FindMedianSortedArrays(int[] nums1, int[] nums2) {
        // Combine both arrays
        int[] mergedArray = new int[nums1.Length + nums2.Length];
        Array.Copy(nums1, mergedArray, nums1.Length);
        Array.Copy(nums2, 0, mergedArray, nums1.Length, nums2.Length);

        // Sort the combined array
        Array.Sort(mergedArray);

        // Finding the median
        int n = mergedArray.Length;
        if (n % 2 == 0) {
            // If length is even, the median is the average of the two middle elements
            return (mergedArray[n / 2 - 1] + mergedArray[n / 2]) / 2.0;
        } else {
            // If length is odd, the median is the middle element
            return mergedArray[n / 2];
        }
    }
}
```

### Time Complexity:
- O((m+n) log(m+n)), where m and n are the sizes of the two arrays due to sorting.

### Space Complexity:
- O(m+n) for storing the merged array.

---

## Binary Search (Optimal)

### Intuition:

To find the median with optimized time complexity, we can use the properties of the median and binary search. Notably, the median divides a sorted set into two equal parts, hence we can use binary search to partition the two arrays such that left half and right half are properly managed.

### Steps:
1. Always binary search on the smaller array to minimize time complexity.
2. Use binary search to partition both arrays such that all element in the left partitions are less than or equal to elements in the right partitions.
3. Adjust partitions based on comparisons and conditions until the correct partitions are found.
4. Calculate the median based on the partitions found.

### Code:

```csharp
public class Solution {
    public double FindMedianSortedArrays(int[] nums1, int[] nums2) {
        // Make sure nums1 is the smaller array
        if (nums1.Length > nums2.Length) {
            return FindMedianSortedArrays(nums2, nums1);
        }

        int x = nums1.Length;
        int y = nums2.Length;

        int low = 0, high = x;
        while (low <= high) {
            int partitionX = (low + high) / 2;
            int partitionY = (x + y + 1) / 2 - partitionX;

            int maxX = (partitionX == 0) ? int.MinValue : nums1[partitionX - 1];
            int minX = (partitionX == x) ? int.MaxValue : nums1[partitionX];

            int maxY = (partitionY == 0) ? int.MinValue : nums2[partitionY - 1];
            int minY = (partitionY == y) ? int.MaxValue : nums2[partitionY];

            if (maxX <= minY && maxY <= minX) {
                // Correct partitioning
                if ((x + y) % 2 == 0) {
                    return (Math.Max(maxX, maxY) + Math.Min(minX, minY)) / 2.0;
                } else {
                    return Math.Max(maxX, maxY);
                }
            } else if (maxX > minY) {
                // Move towards the left in nums1
                high = partitionX - 1;
            } else {
                // Move towards the right in nums1
                low = partitionX + 1;
            }
        }
        
        throw new ArgumentException("Input arrays are not sorted or empty.");
    }
}
```

### Time Complexity:
- O(log(min(m, n))), logarithmic in the smaller array size due to binary search.

### Space Complexity:
- O(1) since no extra space is used, only constant space variables.

This solution is efficient and handles even large inputs gracefully within the constraint limits.

