# [Leetcode Problem 88: Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

## Table of Contents
1. [Approach 1: Merge Then Sort](#approach-1)
2. [Approach 2: Two-Pointer Technique](#approach-2)
3. [Approach 3: In-place Two-Pointer Technique (Optimal)](#approach-3)

## Approach 1: Merge Then Sort <a name="approach-1"></a>

### Intuition
The simplest approach to solve the problem is to first merge the elements of `nums2` into `nums1` and then sort `nums1`. Although it is not the most efficient solution, it works for small inputs and provides an easy starting point.

### Steps
1. Copy all elements from `nums2` into `nums1` starting from index `m`.
2. Sort the array `nums1`.

### Code
```java
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Copy nums2 to nums1 from index m to m+n
        for (int i = 0; i < n; i++) {
            nums1[m + i] = nums2[i];
        }
        // Sort nums1
        Arrays.sort(nums1);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O((m+n)log(m+n)), due to sorting.
- **Space Complexity**: O(1), as we are modifying nums1 in place.

## Approach 2: Two-Pointer Technique <a name="approach-2"></a>

### Intuition
Both arrays `nums1` and `nums2` are sorted, thus we can use a two-pointer technique to merge them efficiently. Create a new array to store the merged sorted elements.

### Steps
1. Initialize three pointers: `p1` for `nums1`, `p2` for `nums2`, and `p` for the new array.
2. Compare elements pointed by `p1` and `p2`, and place the smaller one in the new array.
3. If any elements are left in `nums1` or `nums2`, append them to the end of the new array.
4. Copy the new array back into `nums1`.

### Code
```java
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // New array to store merged result
        int[] sorted = new int[m + n];
        // Pointers for nums1, nums2, and sorted array
        int p1 = 0, p2 = 0, p = 0;
        
        // Compare and merge
        while (p1 < m && p2 < n) {
            if (nums1[p1] <= nums2[p2]) {
                sorted[p++] = nums1[p1++];
            } else {
                sorted[p++] = nums2[p2++];
            }
        }
        
        // Append remaining elements
        while (p1 < m) {
            sorted[p++] = nums1[p1++];
        }
        while (p2 < n) {
            sorted[p++] = nums2[p2++];
        }
        
        // Copy sorted array back to nums1
        System.arraycopy(sorted, 0, nums1, 0, m + n);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(m + n), as we iterate through both arrays once.
- **Space Complexity**: O(m + n), due to the use of an additional array.

## Approach 3: In-place Two-Pointer Technique (Optimal) <a name="approach-3"></a>

### Intuition
Since the space in `nums1` after `m` is unused, leverage this space to place the merged results directly. We'll move backwards from the end of the arrays to avoid overwriting any values.

### Steps
1. Use two pointers `p1` and `p2` starting from the end of the initialized parts of `nums1` and `nums2` respectively, and another pointer `p` from the very end of `nums1`.
2. Compare the current elements of `nums1` and `nums2` and place the larger one at the `p` position.
3. Decrement the respective pointers.
4. If any elements are left in `nums2`, copy them to `nums1` (no need to handle the remaining `nums1`, they are already in place).

### Code
```java
public class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Pointers for nums1, nums2 and the end of merged array
        int p1 = m - 1, p2 = n - 1, p = m + n - 1;
        
        // Merge arrays starting from the end
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
        
        // If there are any remaining elements in nums2, move them to nums1
        while (p2 >= 0) {
            nums1[p--] = nums2[p2--];
        }
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(m + n), as we process each element exactly once.
- **Space Complexity**: O(1), since we are merging in-place without extra space.

In conclusion, the in-place two-pointer technique provides an optimal solution in both time and space complexity, making it the most efficient method among the approaches outlined.

