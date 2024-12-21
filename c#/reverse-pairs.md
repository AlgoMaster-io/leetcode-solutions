### [Leetcode Problem 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

### Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Merge Sort Approach](#merge-sort-approach)

---

#### Brute Force Approach

**Intuition**

The most straightforward way to solve the problem is to check every pair of indices `(i, j)` with `i < j` and count how many pairs `(i, j)` satisfy the condition `nums[i] > 2 * nums[j]`. Since this approach involves checking all pairs, it has higher time complexity, but can serve as a baseline for understanding the problem.

**Code**

```csharp
public class Solution {
    public int ReversePairs(int[] nums) {
        int count = 0;
        // Check each pair (i, j) where i < j
        for (int i = 0; i < nums.Length; i++) {
            for (int j = i + 1; j < nums.Length; j++) {
                // Increment count if the condition is satisfied
                if (nums[i] > 2 * (long)nums[j]) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

**Complexity Analysis**

- **Time Complexity**: O(n²), due to the nested loops iterating over the pairs.
- **Space Complexity**: O(1), as we are not using any extra space other than a few variables.

---

#### Merge Sort Approach

**Intuition**

To optimize the solution, we use the modified merge sort algorithm. The idea is that the merge sort algorithm divides the array into smaller subarrays, sorts them, and combines them back together. During this process, we can count the number of reverse pairs by leveraging sorted subarrays.

1. **Divide** the array into two halves.
2. **Count** reverse pairs in the left half, right half, and the cross pairs (pairs where one element is in the left half and the other in the right half).
3. **Merge** the two halves back together, maintaining sorted order.
4. **Count Cross Pairs**: For cross pairs, for each element in the left half, find how many elements in the right half satisfy the condition using a two-pointer technique.

**Code**

```csharp
public class Solution {
    public int ReversePairs(int[] nums) {
        if (nums == null || nums.Length == 0) return 0;
        return MergeSortAndCount(nums, 0, nums.Length - 1);
    }

    private int MergeSortAndCount(int[] nums, int start, int end) {
        if (start >= end) return 0;
        int mid = start + (end - start) / 2;
        
        int count = MergeSortAndCount(nums, start, mid) + MergeSortAndCount(nums, mid + 1, end);
        
        // Count the cross pairs
        int j = mid + 1;
        for (int i = start; i <= mid; i++) {
            while (j <= end && nums[i] > 2L * nums[j]) {
                j++;
            }
            count += j - (mid + 1);
        }
        
        // Merge the two sorted halves
        Merge(nums, start, mid, end);
        
        return count;
    }

    private void Merge(int[] nums, int start, int mid, int end) {
        int[] temp = new int[end - start + 1];
        int i = start, j = mid + 1, k = 0;
        while (i <= mid && j <= end) {
            if (nums[i] <= nums[j]) {
                temp[k++] = nums[i++];
            } else {
                temp[k++] = nums[j++];
            }
        }
        while (i <= mid) {
            temp[k++] = nums[i++];
        }
        while (j <= end) {
            temp[k++] = nums[j++];
        }
        for (i = start; i <= end; i++) {
            nums[i] = temp[i - start];
        }
    }
}
```

**Complexity Analysis**

- **Time Complexity**: O(n log n), due to the merge sort process.
- **Space Complexity**: O(n), for the temporary array used in merging.

By leveraging the divide and conquer technique, this approach significantly reduces the number of comparisons needed to find the reverse pairs, making it much more efficient than the brute force approach.

