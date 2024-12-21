
[Leetcode Problem 4: Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Binary Search Approach](#optimized-binary-search-approach)

### Brute Force Approach

#### Intuition
The simplest approach to find the median of two sorted arrays is to merge them into a single sorted array, then directly find the median. This approach leverages the idea of merging two arrays, similar to the merge process in merge sort.

#### Steps:
1. Initialize an array to hold the merged elements of the two input arrays.
2. Use two pointers to iterate over both arrays and append the smaller element from the two into the merged array.
3. If one array is exhausted, append the remaining elements of the other array.
4. Calculate the median from the merged array:
   - If the size of the merged array is odd, the median is the middle element.
   - If the size is even, the median is the average of the two middle elements.

#### Java Code
```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] mergedArray = new int[nums1.length + nums2.length];
        int i = 0, j = 0, k = 0;

        // Merge the two arrays
        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] <= nums2[j]) {
                mergedArray[k++] = nums1[i++];
            } else {
                mergedArray[k++] = nums2[j++];
            }
        }

        // Check for remaining elements
        while (i < nums1.length) {
            mergedArray[k++] = nums1[i++];
        }
        while (j < nums2.length) {
            mergedArray[k++] = nums2[j++];
        }

        // Find the median
        int n = mergedArray.length;
        if (n % 2 == 0) {
            return (mergedArray[n / 2 - 1] + mergedArray[n / 2]) / 2.0;
        } else {
            return mergedArray[n / 2];
        }
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: O(m + n), where m and n are the lengths of the two arrays. This is because we iterate through both arrays once.
- **Space Complexity**: O(m + n) due to the creation of the merged array.

### Optimized Binary Search Approach

#### Intuition
A more efficient approach uses binary search. Instead of merging the entire arrays, we use binary search on the shorter array to partition the two arrays into two halves such that all elements on the left side are less than or equal to all elements on the right side. This way, we can determine the median in O(log(min(m, n))) time.

#### Steps:
1. Make sure to always use binary search on the smaller array to minimize the time complexity.
2. Use binary search to find the correct partition:
   - Divide both arrays into two halves such that the maximum element on the left is less than or equal to the minimum element on the right from both arrays.
3. Once the correct partition is found:
   - If the combined length of arrays is even, the median is the average of the maximum element of the left part and the minimum element of the right part.
   - If it's odd, the median is the maximum element of the left part.

#### Java Code
```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        // Ensure nums1 is the smaller array
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int x = nums1.length;
        int y = nums2.length;
        int low = 0, high = x;

        while (low <= high) {
            int partitionX = (low + high) / 2;
            int partitionY = (x + y + 1) / 2 - partitionX;

            int maxX = (partitionX == 0) ? Integer.MIN_VALUE : nums1[partitionX - 1];
            int minX = (partitionX == x) ? Integer.MAX_VALUE : nums1[partitionX];

            int maxY = (partitionY == 0) ? Integer.MIN_VALUE : nums2[partitionY - 1];
            int minY = (partitionY == y) ? Integer.MAX_VALUE : nums2[partitionY];

            if (maxX <= minY && maxY <= minX) {
                // Correct partition
                if ((x + y) % 2 == 0) {
                    return ((double)Math.max(maxX, maxY) + Math.min(minX, minY)) / 2;
                } else {
                    return (double)Math.max(maxX, maxY);
                }
            } else if (maxX > minY) {
                high = partitionX - 1; // Adjust partition to the left
            } else {
                low = partitionX + 1; // Adjust partition to the right
            }
        }
        
        throw new IllegalArgumentException("Input arrays are not sorted");
    }
}
```

#### Time and Space Complexity
- **Time Complexity**: O(log(min(m, n))), where m and n are the lengths of two input arrays. The binary search is applied on the smaller array.
- **Space Complexity**: O(1), as no additional space is used other than variable storage.

