# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int SubarraysDivByK(int[] nums, int k) {
        int count = 0;

        // Check all subarrays
        for (int i = 0; i < nums.Length; i++) {
            int sum = 0;

            for (int j = i; j < nums.Length; j++) {
                sum += nums[j];

                // Check if the sum is divisible by k
                if (sum % k == 0) {
                    count++;
                }
            }
        }

        return count;
    }
}
```

## Approach 2: Prefix Sum with HashMap (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(k)
using System.Collections.Generic;

public class Solution {
    public int SubarraysDivByK(int[] nums, int k) {
        Dictionary<int, int> remainderMap = new Dictionary<int, int>();
        remainderMap[0] = 1; // Initialize with a remainder of 0

        int count = 0;
        int sum = 0;

        foreach (int num in nums) {
            sum += num;
            int remainder = sum % k;

            // Normalize the remainder to always be non-negative
            if (remainder < 0) {
                remainder += k;
            }

            // If the remainder exists in the map, add its frequency to the count
            if (remainderMap.ContainsKey(remainder)) {
                count += remainderMap[remainder];
            }

            // Update the frequency of the current remainder
            if (remainderMap.ContainsKey(remainder)) {
                remainderMap[remainder]++;
            } else {
                remainderMap[remainder] = 1;
            }
        }

        return count;
    }
}
```

