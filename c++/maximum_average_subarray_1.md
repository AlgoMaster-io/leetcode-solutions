# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approach 1: Brute Force (Basic)

### Solution
```cpp
// Time Complexity: O(n * k)
// Space Complexity: O(1)
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double maxAverage = -DBL_MAX;

        // Check all subarrays of size k
        for (int i = 0; i <= nums.size() - k; i++) {
            double sum = 0;

            // Calculate the sum of the current subarray
            for (int j = i; j < i + k; j++) {
                sum += nums[j];
            }

            // Update the maximum average
            maxAverage = max(maxAverage, sum / k);
        }

        return maxAverage;
    }
};
```

## Approach 2: Sliding Window (Optimal)

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double sum = 0;

        // Calculate the sum of the first window
        for (int i = 0; i < k; i++) {
            sum += nums[i];
        }

        double maxAverage = sum / k;

        // Slide the window over the array
        for (int i = k; i < nums.size(); i++) {
            sum += nums[i] - nums[i - k]; // Update the sum for the new window
            maxAverage = max(maxAverage, sum / k); // Update the maximum average
        }

        return maxAverage;
    }
};
```

