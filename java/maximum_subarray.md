# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = Integer.MIN_VALUE;

        // Check all possible subarrays
        for (int i = 0; i < nums.length; i++) {
            int currentSum = 0;

            for (int j = i; j < nums.length; j++) {
                currentSum += nums[j];
                maxSum = Math.max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
}
```

## Approach 2: Kadane’s Algorithm (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = nums[0];
        int currentSum = nums[0];

        // Traverse the array
        for (int i = 1; i < nums.length; i++) {
            // Update the current sum: either extend the subarray or start a new one
            currentSum = Math.max(nums[i], currentSum + nums[i]);

            // Update the maximum sum encountered so far
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }
}
```