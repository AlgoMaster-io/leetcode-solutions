# [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int SubarraySum(int[] nums, int k) {
        int count = 0;

        // Check all subarrays
        for (int i = 0; i < nums.Length; i++) {
            int sum = 0;

            for (int j = i; j < nums.Length; j++) {
                sum += nums[j];

                // If the sum equals k, increment the count
                if (sum == k) {
                    count++;
                }
            }
        }

        return count;
    }
}
```

## Approach 2: Prefix Sum with Dictionary (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int SubarraySum(int[] nums, int k) {
        var prefixSumMap = new Dictionary<int, int>();
        prefixSumMap[0] = 1; // Initialize with a prefix sum of 0

        int count = 0;
        int sum = 0;

        foreach (int num in nums) {
            sum += num; // Update the prefix sum

            // Check if there's a prefix sum that makes the current sum equal to k
            if (prefixSumMap.ContainsKey(sum - k)) {
                count += prefixSumMap[sum - k];
            }

            // Update the frequency of the current prefix sum
            if (!prefixSumMap.ContainsKey(sum)) {
                prefixSumMap[sum] = 0;
            }
            prefixSumMap[sum]++;
        }

        return count;
    }
}
```

