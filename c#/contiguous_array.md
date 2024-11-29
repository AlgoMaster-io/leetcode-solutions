# [525. Contiguous Array](https://leetcode.com/problems/contiguous-array/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int FindMaxLength(int[] nums) {
        int maxLength = 0;

        // Check all possible subarrays
        for (int i = 0; i < nums.Length; i++) {
            int count = 0;

            for (int j = i; j < nums.Length; j++) {
                // Increment or decrement count based on value
                count += nums[j] == 1 ? 1 : -1;

                // If count is zero, it means equal number of 0s and 1s
                if (count == 0) {
                    maxLength = Math.Max(maxLength, j - i + 1);
                }
            }
        }

        return maxLength;
    }
}
```

## Approach 2: HashMap (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int FindMaxLength(int[] nums) {
        var map = new Dictionary<int, int>();
        map[0] = -1; // Initialize with a count of 0 at index -1
        int maxLength = 0;
        int count = 0;

        for (int i = 0; i < nums.Length; i++) {
            // Increment or decrement count based on value
            count += nums[i] == 1 ? 1 : -1;

            if (map.ContainsKey(count)) {
                // Calculate the length of the subarray
                maxLength = Math.Max(maxLength, i - map[count]);
            } else {
                // Store the first occurrence of this count
                map[count] = i;
            }
        }

        return maxLength;
    }
}
```

