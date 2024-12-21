# [Leetcode 493: Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Merge Sort with Modification](#approach-2-merge-sort-with-modification)

---

## Approach 1: Brute Force

### Intuition
The problem asks us to count the number of important reverse pairs `(i, j)` such that `i < j` and `nums[i] > 2 * nums[j]`. A straightforward approach is to evaluate every possible pair `(i, j)` and check the condition. This leads to a simple double loop solution.

### Algorithm
1. Iterate over each pair `(i, j)` where `i < j`.
2. For each pair, check if `nums[i] > 2 * nums[j]`.
3. Count all such pairs.

### Code

```java
public class Solution {
    public int reversePairs(int[] nums) {
        int n = nums.length;
        int count = 0;
        
        // Loop over each pair (i, j)
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                // Check the condition nums[i] > 2 * nums[j]
                if (nums[i] > 2L * nums[j]) {
                    count++;
                }
            }
        }
        return count;
    }
}
```

### Time Complexity
- O(n²), where `n` is the number of elements in `nums`. We check each pair `(i, j)` once.

### Space Complexity
- O(1), as we are utilizing only a constant amount of extra space.

---

## Approach 2: Merge Sort with Modification

### Intuition
The brute-force solution is inefficient for large inputs. By using a modified version of merge sort, we can efficiently count reverse pairs. The essential insight is to leverage the sorted order of the array to count pairs more efficiently.

### Algorithm
1. Use merge sort to sort the array.
2. During the merge process, for each left element, count how many elements in the right sorted array satisfy the reverse pair condition.
3. Count and merge both parts of the array while maintaining the order.

### Code

```java
public class Solution {
    public int reversePairs(int[] nums) {
        if (nums == null || nums.length < 2) {
            return 0;
        }
        return mergeSort(nums, 0, nums.length - 1);
    }
    
    private int mergeSort(int[] nums, int left, int right) {
        if (left >= right) return 0;
        
        int mid = left + (right - left) / 2;
        int count = mergeSort(nums, left, mid) + mergeSort(nums, mid + 1, right);
        
        int j = mid + 1;
        // Count important reverse pairs
        for (int i = left; i <= mid; i++) {
            while (j <= right && nums[i] > 2L * nums[j]) {
                j++;
            }
            count += j - (mid + 1);
        }
        
        // Merge the two halves
        merge(nums, left, mid, right);
        return count;
    }
    
    private void merge(int[] nums, int left, int mid, int right) {
        int[] temp = new int[right - left + 1];
        int i = left, j = mid + 1, k = 0;
        
        while (i <= mid && j <= right) {
            // Choose the smaller of the two elements to maintain the order
            if (nums[i] <= nums[j]) {
                temp[k++] = nums[i++];
            } else {
                temp[k++] = nums[j++];
            }
        }
        
        // Copy remaining elements from the left half
        while (i <= mid) {
            temp[k++] = nums[i++];
        }
        
        // Copy remaining elements from the right half
        while (j <= right) {
            temp[k++] = nums[j++];
        }
        
        // Copy back to the original array
        System.arraycopy(temp, 0, nums, left, temp.length);
    }
}
```

### Time Complexity
- O(n log n), where `n` is the number of elements in `nums`. This includes splitting the array (`log n` levels) and merging subarrays (`O(n)` per level).

### Space Complexity
- O(n), due to the temporary arrays used during merge steps.

