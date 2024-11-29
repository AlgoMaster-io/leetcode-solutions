# [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approach 1: Merge Sort (Divide and Conquer)

### Solution
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
public class Solution {
    public int ReversePairs(int[] nums) {
        if (nums == null || nums.Length < 2) {
            return 0;
        }
        return MergeSort(nums, 0, nums.Length - 1);
    }

    private int MergeSort(int[] nums, int left, int right) {
        if (left >= right) {
            return 0;
        }

        int mid = left + (right - left) / 2;

        // Count reverse pairs in left and right halves, and across them
        int count = MergeSort(nums, left, mid) + MergeSort(nums, mid + 1, right);

        // Count cross reverse pairs
        int j = mid + 1;
        for (int i = left; i <= mid; i++) {
            while (j <= right && (long) nums[i] > 2L * nums[j]) {
                j++;
            }
            count += (j - mid - 1);
        }

        // Merge the two halves
        Merge(nums, left, mid, right);

        return count;
    }

    private void Merge(int[] nums, int left, int mid, int right) {
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

        Array.Copy(temp, 0, nums, left, temp.Length);
    }
}
```

## Approach 2: Binary Indexed Tree (Fenwick Tree)

### Solution
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;
using System.Collections.Generic;

public class Solution {
    public int ReversePairs(int[] nums) {
        // Coordinate compression
        SortedSet<long> set = new SortedSet<long>();
        foreach (int num in nums) {
            set.Add((long)num);
            set.Add((long)num * 2);
        }

        Dictionary<long, int> map = new Dictionary<long, int>();
        int rank = 1;
        foreach (long num in set) {
            map[num] = rank++;
        }

        // Binary Indexed Tree
        int[] bit = new int[rank];
        int count = 0;

        for (int i = nums.Length - 1; i >= 0; i--) {
            // Count elements smaller than nums[i]
            count += Query(bit, map[(long)nums[i]] - 1);

            // Add nums[i] * 2 to the BIT
            Update(bit, map[(long)nums[i] * 2], 1);
        }

        return count;
    }

    private void Update(int[] bit, int index, int delta) {
        while (index < bit.Length) {
            bit[index] += delta;
            index += index & -index;
        }
    }

    private int Query(int[] bit, int index) {
        int sum = 0;
        while (index > 0) {
            sum += bit[index];
            index -= index & -index;
        }
        return sum;
    }
}
```

