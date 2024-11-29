# [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)

public class Solution {
    public bool CheckSubarraySum(int[] nums, int k) {
        int n = nums.Length;

        // Check every possible subarray
        for (int i = 0; i < n - 1; i++) {
            int sum = nums[i];
            for (int j = i + 1; j < n; j++) {
                sum += nums[j];
                if (k == 0 ? sum == 0 : sum % k == 0) {
                    return true; // Subarray sum is a multiple of k
                }
            }
        }

        return false; // No valid subarray found
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
    public bool CheckSubarraySum(int[] nums, int k) {
        Dictionary<int, int> remainderMap = new Dictionary<int, int>();
        remainderMap[0] = -1; // Initialize with a remainder of 0 at index -1
        int sum = 0;

        for (int i = 0; i < nums.Length; i++) {
            sum += nums[i];

            // Compute the remainder when divided by k
            int remainder = k == 0 ? sum : sum % k;

            // If the remainder already exists
            if (remainderMap.ContainsKey(remainder)) {
                // Check if the subarray length is at least 2
                if (i - remainderMap[remainder] > 1) {
                    return true;
                }
            } else {
                // Store the index of this remainder
                remainderMap[remainder] = i;
            }
        }

        return false; // No valid subarray found
    }
}
```

