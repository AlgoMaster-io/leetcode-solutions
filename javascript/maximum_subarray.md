# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
javascript
```javascript
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
    maxSubArray(nums) {
        let maxSum = Number.MIN_SAFE_INTEGER;

        // Check all possible subarrays
        for (let i = 0; i < nums.length; i++) {
            let currentSum = 0;

            for (let j = i; j < nums.length; j++) {
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
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    maxSubArray(nums) {
        let maxSum = nums[0];
        let currentSum = nums[0];

        // Traverse the array
        for (let i = 1; i < nums.length; i++) {
            // Update the current sum: either extend the subarray or start a new one
            currentSum = Math.max(nums[i], currentSum + nums[i]);

            // Update the maximum sum encountered so far
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
    }
}
```

