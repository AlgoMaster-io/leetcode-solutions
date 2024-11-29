# 300. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

## Approach 1: Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n)
public class Solution {
    public int LengthOfLIS(int[] nums) {
        if (nums == null || nums.Length == 0) {
            return 0;
        }

        int[] dp = new int[nums.Length]; // dp array to store the LIS ending at each index
        int maxLength = 1; // Minimum LIS length is 1 (a single element)

        // Initialize all dp values to 1
        for (int i = 0; i < nums.Length; i++) {
            dp[i] = 1;
        }

        // Build the dp array
        for (int i = 1; i < nums.Length; i++) { 
            for (int j = 0; j < i; j++) { 
                if (nums[i] > nums[j]) { // Check if nums[i] can extend the subsequence at nums[j]
                    dp[i] = Math.Max(dp[i], dp[j] + 1); // Update dp[i] with the maximum length found
                }
            }
            maxLength = Math.Max(maxLength, dp[i]); // Update the overall LIS length
        }

        return maxLength;
    }
}
```

## Approach 2: Binary Search with Patience Sorting

### Solution
csharp
```csharp
// Time Complexity: O(n log n)
// Space Complexity: O(n)
using System;

public class Solution {
    public int LengthOfLIS(int[] nums) {
        if (nums == null || nums.Length == 0) {
            return 0;
        }

        int[] tails = new int[nums.Length]; // Array to store the smallest tail for all increasing subsequences
        int size = 0; // Size of the effective array

        foreach (int num in nums) {
            int i = 0, j = size; // Binary search for the insertion point of num in tails

            while (i < j) {
                int mid = (i + j) / 2;
                if (tails[mid] < num) {
                    i = mid + 1;
                } else {
                    j = mid;
                }
            }

            tails[i] = num; // Replace or extend the increasing subsequence
            if (i == size) {
                size++; // Increment size if a new subsequence tail is created
            }
        }

        return size;
    }
}
```


