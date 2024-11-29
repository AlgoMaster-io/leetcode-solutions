# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int MaxSubarraySumCircular(int[] nums) {
        int n = nums.Length;
        int maxSum = int.MinValue;

        // Iterate over all possible subarrays, including circular ones
        for (int i = 0; i < n; i++) {
            int currentSum = 0;

            for (int j = 0; j < n; j++) {
                currentSum += nums[(i + j) % n];
                maxSum = Math.Max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```

## Approach 2: Optimized with Kadane’s Algorithm

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int MaxSubarraySumCircular(int[] nums) {
        int totalSum = 0;
        int maxSum = nums[0], currentMax = 0;
        int minSum = nums[0], currentMin = 0;

        foreach (int num in nums) {
            // Calculate maximum subarray sum using Kadane's algorithm
            currentMax = Math.Max(num, currentMax + num);
            maxSum = Math.Max(maxSum, currentMax);

            // Calculate minimum subarray sum using Kadane's algorithm
            currentMin = Math.Min(num, currentMin + num);
            minSum = Math.Min(minSum, currentMin);

            // Calculate total sum of the array
            totalSum += num;
        }

        // If all numbers are negative, return the maximum sum (non-circular)
        return maxSum > 0 ? Math.Max(maxSum, totalSum - minSum) : maxSum;
    }
}
```

