# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Sliding Window Approach](#sliding-window-approach)

## Brute Force Approach

### Intuition:
The brute force solution is straightforward: iterate over all possible subarrays of length `k` and calculate their averages. Return the maximum average found.

### Detailed Explanation:
1. Iterate over each starting point of the subarray from `0` to `n-k`.
2. For each starting point, calculate the sum of the next `k` elements.
3. Compute the average from the sum and update the maximum average if the current one is greater.
4. Repeat the process for the remaining elements.

### Code:

```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double maxAverage = INT_MIN;

        for (int i = 0; i <= nums.size() - k; ++i) {
            double sum = 0;

            // Calculate sum of subarray starting at index i
            for (int j = i; j < i + k; ++j) {
                sum += nums[j];
            }

            // Calculate the average
            double average = sum / k;

            // Update maxAverage if the current average is greater
            if (average > maxAverage) {
                maxAverage = average;
            }
        }
        
        return maxAverage;
    }
};
```

### Time Complexity:
- O(n * k), where `n` is the number of elements in `nums`. We calculate the sum of a subarray `k` times for each starting position.

### Space Complexity:
- O(1), no extra space is used beyond input variables.

## Sliding Window Approach

### Intuition:
Instead of recalculating the entire sum for every subarray, use the sliding window technique to maintain the sum of the current subarray of length `k` and update it as the window slides.

### Detailed Explanation:
1. Calculate the sum of the initial subarray of length `k`.
2. Set this as the initial maximum sum.
3. Slide the window one element at a time:
   - Subtract the first element of the previous window.
   - Add the new element from the current window.
   - Update the maximum average based on the new sum.
4. This allows maintaining a running sum without recalculating it from scratch each time.

### Code:

```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double currentSum = 0;

        // Calculate the sum of the first 'k' elements
        for (int i = 0; i < k; ++i) {
            currentSum += nums[i];
        }

        // Initialize maxSum with the first window's sum
        double maxSum = currentSum;

        // Slide the window over the rest of the elements
        for (int i = k; i < nums.size(); ++i) {
            // Update currentSum to reflect the sliding window
            currentSum = currentSum - nums[i - k] + nums[i];
            // Update the maxSum if currentSum is greater
            if (currentSum > maxSum) {
                maxSum = currentSum;
            }
        }
        
        // Divide by 'k' to get the maximum average
        return maxSum / k;
    }
};
```

### Time Complexity:
- O(n), where `n` is the number of elements in `nums`, since we only pass through the array once.

### Space Complexity:
- O(1), as only a fixed amount of extra memory is used.

