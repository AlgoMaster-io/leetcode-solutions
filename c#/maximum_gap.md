# [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)

## Approach 1: Sorting

### Solution
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(1) (ignoring the space required for sorting)
using System;

public class Solution {
    public int MaximumGap(int[] nums) {
        if (nums == null || nums.Length < 2) {
            return 0;
        }

        // Sort the array
        Array.Sort(nums);

        int maxGap = 0;

        // Calculate the maximum gap between consecutive elements
        for (int i = 1; i < nums.Length; i++) {
            maxGap = Math.Max(maxGap, nums[i] - nums[i - 1]);
        }

        return maxGap;
    }
}
```

## Approach 2: Bucket Sort (Using Pigeonhole Principle)

### Solution
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;

public class Solution {
    public int MaximumGap(int[] nums) {
        if (nums == null || nums.Length < 2) {
            return 0;
        }

        int min = int.MaxValue, max = int.MinValue;
        foreach (int num in nums) {
            min = Math.Min(min, num);
            max = Math.Max(max, num);
        }

        // Calculate the gap size and number of buckets
        int n = nums.Length;
        int gap = (int)Math.Ceiling((double)(max - min) / (n - 1));
        int[] bucketMin = new int[n - 1];
        int[] bucketMax = new int[n - 1];
        Array.Fill(bucketMin, int.MaxValue);
        Array.Fill(bucketMax, int.MinValue);

        // Place each number into a bucket
        foreach (int num in nums) {
            if (num == min || num == max) {
                continue;
            }
            int index = (num - min) / gap;
            bucketMin[index] = Math.Min(bucketMin[index], num);
            bucketMax[index] = Math.Max(bucketMax[index], num);
        }

        // Calculate the maximum gap
        int maxGap = 0, previous = min;
        for (int i = 0; i < n - 1; i++) {
            if (bucketMin[i] == int.MaxValue) {
                continue; // Skip empty buckets
            }
            maxGap = Math.Max(maxGap, bucketMin[i] - previous);
            previous = bucketMax[i];
        }
        maxGap = Math.Max(maxGap, max - previous); // Final gap with the max element

        return maxGap;
    }
}
```

