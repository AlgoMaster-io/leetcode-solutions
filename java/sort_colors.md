# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approach 1: Two-Pass Counting Sort

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public void sortColors(int[] nums) {
        int[] count = new int[3];

        // Count the occurrences of 0, 1, and 2
        for (int num : nums) {
            count[num]++;
        }

        // Overwrite the array based on the counts
        int index = 0;
        for (int i = 0; i < 3; i++) {
            while (count[i] > 0) {
                nums[index++] = i;
                count[i]--;
            }
        }
    }
}
```

## Approach 2: One-Pass (Dutch National Flag Algorithm)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public void sortColors(int[] nums) {
        int low = 0, mid = 0, high = nums.length - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                // Swap nums[low] and nums[mid] and move pointers
                swap(nums, low++, mid++);
            } else if (nums[mid] == 1) {
                // Just move the mid pointer
                mid++;
            } else {
                // Swap nums[mid] and nums[high] and move high pointer
                swap(nums, mid, high--);
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```