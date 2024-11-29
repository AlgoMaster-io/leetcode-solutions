# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int MaxSubArray(int[] nums) {
        int maxSum = int.MinValue;

        // Check all possible subarrays
        for (int i = 0; i < nums.Length; i++) {
            int currentSum = 0;

            for (int j = i; j < nums.Length; j++) {
                currentSum += nums[j];
                maxSum = Math.Max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```

## Approach 2: Kadane’s Algorithm (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int MaxSubArray(int[] nums) {
        int maxSum = nums[0];
        int currentSum = nums[0];

        // Traverse the array
        for (int i = 1; i < nums.Length; i++) {
            // Update the current sum: either extend the subarray or start a new one
            currentSum = Math.Max(nums[i], currentSum + nums[i]);

            // Update the maximum sum encountered so far
            maxSum = Math.Max(maxSum, currentSum);
        }

        return maxSum;
    }
}
```

