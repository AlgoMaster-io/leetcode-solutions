# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Approach 1: Using Built-in Sorting (Simplest)

### Solution
```java
// Time Complexity: O((m + n) log(m + n))
// Space Complexity: O(1)
import java.util.Arrays;

public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Copy nums2 into nums1
        System.arraycopy(nums2, 0, nums1, m, n);

        // Sort the combined array
        Arrays.sort(nums1);
    }
}
```

## Approach 2: Two Pointers with Temporary Array

### Solution
```java
// Time Complexity: O(m + n)
// Space Complexity: O(m + n)
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] temp = new int[m + n];
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
        System.arraycopy(temp, 0, nums1, 0, m + n);
    }
}
```

## Approach 3: Two Pointers from End (Optimal)

### Solution
```java
// Time Complexity: O(m + n)
// Space Complexity: O(1)
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
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
}
```