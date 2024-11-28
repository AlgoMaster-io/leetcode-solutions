# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
public class Solution {
    public int reversePairs(int[] nums) {
        if (nums == null || nums.length < 2) {
            return 0;
        }
        return mergeSort(nums, 0, nums.length - 1);
    }

    private int mergeSort(int[] nums, int left, int right) {
        if (left >= right) {
            return 0;
        }

        int mid = left + (right - left) / 2;

        // Count reverse pairs in left and right halves, and across them
        int count = mergeSort(nums, left, mid) + mergeSort(nums, mid + 1, right);

        // Count cross reverse pairs
        int j = mid + 1;
        for (int i = left; i <= mid; i++) {
            while (j <= right && (long) nums[i] > 2L * nums[j]) {
                j++;
            }
            count += (j - mid - 1);
        }

        // Merge the two halves
        merge(nums, left, mid, right);

        return count;
    }

    private void merge(int[] nums, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];
        int i = left, j = mid + 1, k = 0;

        while (i <= mid && j <= right) {
            if (nums[i] <= nums[j]) {
                temp[k++] = nums[i++];
            } else {
                temp[k++] = nums[j++];
            }
        }

        while (i <= mid) {
            temp[k++] = nums[i++];
        }

        while (j <= right) {
            temp[k++] = nums[j++];
        }

        System.arraycopy(temp, 0, nums, left, temp.length);
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(n)
import java.util.*;

public class Solution {
    public int reversePairs(int[] nums) {
        // Coordinate compression
        TreeSet<Long> set = new TreeSet<>();
        for (int num : nums) {
            set.add((long) num);
            set.add((long) num * 2);
        }

        Map<Long, Integer> map = new HashMap<>();
        int rank = 1;
        for (long num : set) {
            map.put(num, rank++);
        }

        // Binary Indexed Tree
        int[] bit = new int[rank];
        int count = 0;

        for (int i = nums.length - 1; i >= 0; i--) {
            // Count elements smaller than nums[i]
            count += query(bit, map.get((long) nums[i]) - 1);

            // Add nums[i] * 2 to the BIT
            update(bit, map.get((long) nums[i] * 2), 1);
        }

        return count;
    }

    private void update(int[] bit, int index, int delta) {
        while (index < bit.length) {
            bit[index] += delta;
            index += index & -index;
        }
    }

    private int query(int[] bit, int index) {
        int sum = 0;
        while (index > 0) {
            sum += bit[index];
            index -= index & -index;
        }
        return sum;
    }
}
```