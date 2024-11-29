# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

## Approach 1: Two-Pass Counting Sort

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public void SortColors(int[] nums) {
        int[] count = new int[3];

        // Count the occurrences of 0, 1, and 2
        foreach (int num in nums) {
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
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public void SortColors(int[] nums) {
        int low = 0, mid = 0, high = nums.Length - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                // Swap nums[low] and nums[mid] and move pointers
                Swap(nums, low++, mid++);
            } else if (nums[mid] == 1) {
                // Just move the mid pointer
                mid++;
            } else {
                // Swap nums[mid] and nums[high] and move high pointer
                Swap(nums, mid, high--);
            }
        }
    }

    private void Swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

