# [918. Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int n = nums.size();
        int maxSum = INT_MIN;

        // Iterate over all possible subarrays, including circular ones
        for (int i = 0; i < n; i++) {
            int currentSum = 0;

            for (int j = 0; j < n; j++) {
                currentSum += nums[(i + j) % n];
                maxSum = max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
};
```

## Approach 2: Optimized with Kadane’s Algorithm

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int totalSum = 0;
        int maxSum = nums[0], currentMax = 0;
        int minSum = nums[0], currentMin = 0;

        for (int num : nums) {
            // Calculate maximum subarray sum using Kadane's algorithm
            currentMax = max(num, currentMax + num);
            maxSum = max(maxSum, currentMax);

            // Calculate minimum subarray sum using Kadane's algorithm
            currentMin = min(num, currentMin + num);
            minSum = min(minSum, currentMin);

            // Calculate total sum of the array
            totalSum += num;
        }

        // If all numbers are negative, return the maximum sum (non-circular)
        return maxSum > 0 ? max(maxSum, totalSum - minSum) : maxSum;
    }
};
```

