# [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approach 1: Sorting

### Solution
```java
// Time Complexity: O(n log n)
// Space Complexity: O(1) (ignoring the space required for sorting)
import java.util.Arrays;

public class Solution {
    public int maximumGap(int[] nums) {
        if (nums == null || nums.length < 2) {
            return 0;
        }

        // Sort the array
        Arrays.sort(nums);

        int maxGap = 0;

        // Calculate the maximum gap between consecutive elements
        for (int i = 1; i < nums.length; i++) {
            maxGap = Math.max(maxGap, nums[i] - nums[i - 1]);
        }

        return maxGap;
    }
}
```

## Approach 2: Bucket Sort (Using Pigeonhole Principle)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
import java.util.Arrays;

public class Solution {
    public int maximumGap(int[] nums) {
        if (nums == null || nums.length < 2) {
            return 0;
        }

        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        for (int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }

        // Calculate the gap size and number of buckets
        int n = nums.length;
        int gap = (int) Math.ceil((double) (max - min) / (n - 1));
        int[] bucketMin = new int[n - 1];
        int[] bucketMax = new int[n - 1];
        Arrays.fill(bucketMin, Integer.MAX_VALUE);
        Arrays.fill(bucketMax, Integer.MIN_VALUE);

        // Place each number into a bucket
        for (int num : nums) {
            if (num == min || num == max) {
                continue;
            }
            int index = (num - min) / gap;
            bucketMin[index] = Math.min(bucketMin[index], num);
            bucketMax[index] = Math.max(bucketMax[index], num);
        }

        // Calculate the maximum gap
        int maxGap = 0, previous = min;
        for (int i = 0; i < n - 1; i++) {
            if (bucketMin[i] == Integer.MAX_VALUE) {
                continue; // Skip empty buckets
            }
            maxGap = Math.max(maxGap, bucketMin[i] - previous);
            previous = bucketMax[i];
        }
        maxGap = Math.max(maxGap, max - previous); // Final gap with the max element

        return maxGap;
    }
}
```