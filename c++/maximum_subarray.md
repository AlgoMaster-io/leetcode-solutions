# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = INT_MIN;

        // Check all possible subarrays
        for (int i = 0; i < nums.size(); i++) {
            int currentSum = 0;

            for (int j = i; j < nums.size(); j++) {
                currentSum += nums[j];
                maxSum = max(maxSum, currentSum);
            }
        }

        return maxSum;
    }
};
```

## Approach 2: Kadane’s Algorithm (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0];
        int currentSum = nums[0];

        // Traverse the array
        for (int i = 1; i < nums.size(); i++) {
            // Update the current sum: either extend the subarray or start a new one
            currentSum = max(nums[i], currentSum + nums[i]);

            // Update the maximum sum encountered so far
            maxSum = max(maxSum, currentSum);
        }

        return maxSum;
    }
};
```

