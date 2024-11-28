# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int n = nums.length;
        int maxSum = Integer.MIN_VALUE;

        // Iterate over all possible subarrays, including circular ones
        for (int i = 0; i < n; i++) {
            int currentSum = 0;

            for (int j = 0; j < n; j++) {
                currentSum += nums[(i + j) % n];
                maxSum = Math.max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```

## Approach 2: Optimized with Kadane’s Algorithm

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int maxSubarraySumCircular(int[] nums) {
        int totalSum = 0;
        int maxSum = nums[0], currentMax = 0;
        int minSum = nums[0], currentMin = 0;

        for (int num : nums) {
            // Calculate maximum subarray sum using Kadane's algorithm
            currentMax = Math.max(num, currentMax + num);
            maxSum = Math.max(maxSum, currentMax);

            // Calculate minimum subarray sum using Kadane's algorithm
            currentMin = Math.min(num, currentMin + num);
            minSum = Math.min(minSum, currentMin);

            // Calculate total sum of the array
            totalSum += num;
        }

        // If all numbers are negative, return the maximum sum (non-circular)
        return maxSum > 0 ? Math.max(maxSum, totalSum - minSum) : maxSum;
    }
}
```