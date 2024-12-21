# [Leetcode 643: Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Solutions:
1. [Basic Brute Force Approach](#basic-brute-force-approach)
2. [Sliding Window Approach](#sliding-window-approach)

### Basic Brute Force Approach

**Intuition:**

The basic idea for this problem is to consider all possible contiguous subarrays of size `k` within the array and compute their averages. The maximum average amongst all is our desired result. This is a straightforward approach but has a higher time complexity due to the nested loops.

**Approach:**

1. Initialize `maxAverage` to the smallest possible double value to keep track of the maximum average found.
2. Loop through starting index `i` from `0` to `nums.Length - k`.
3. For each starting index `i`, calculate the sum of the subarray of length `k`.
4. Compute the average of these `k` elements.
5. If this average is greater than the maximum average recorded so far, update `maxAverage`.
6. Return the maximum average found.

**Time Complexity:** O(n*k), where `n` is the length of the array `nums`.

**Space Complexity:** O(1)

```csharp
public class Solution {
    public double FindMaxAverage(int[] nums, int k) {
        // Initialize max average to a very small number
        double maxAverage = double.MinValue;
        
        // Loop to consider every subarray of size k
        for (int i = 0; i <= nums.Length - k; i++) {
            int sum = 0;
            // Compute sum of subarray of size k starting from i
            for (int j = i; j < i + k; j++) {
                sum += nums[j];
            }
            // Calculate the average of the current subarray
            double average = (double)sum / k;
            // Update maximum average if this average is greater
            if (average > maxAverage) {
                maxAverage = average;
            }
        }
        
        return maxAverage;
    }
}
```

### Sliding Window Approach

**Intuition:**

Instead of recalculating the sum for each subarray of length `k`, use previously calculated results to optimize. By maintaining a sum of the current window and adjusting it as the window slides, this problem becomes more efficient.

**Approach:**

1. Calculate the sum of the first `k` elements.
2. Set this as the initial `maxSum`.
3. Slide the window by one element until you reach the end of the array:
   - Subtract the element going out of the window (leftmost).
   - Add the next element coming into the window (rightmost).
   - Update `maxSum` if the current sum is greater.
4. Return the `maxSum` divided by `k` to get the maximum average.

**Time Complexity:** O(n), where `n` is the length of the array `nums`.

**Space Complexity:** O(1)

```csharp
public class Solution {
    public double FindMaxAverage(int[] nums, int k) {
        // Calculate sum of the first k elements
        int sum = 0;
        for (int i = 0; i < k; i++) {
            sum += nums[i];
        }
        
        // Initialize maxSum as the initially calculated sum
        int maxSum = sum;
        
        // Slide the window over the rest of the array
        for (int i = k; i < nums.Length; i++) {
            // Adjust the sum by removing the element going out of the window
            // and adding the one coming into the window
            sum = sum - nums[i - k] + nums[i];
            // Update maxSum if current window's sum is larger
            if (sum > maxSum) {
                maxSum = sum;
            }
        }
        
        // Calculate the maximum average by dividing maxSum by k
        return (double)maxSum / k;
    }
}
```

This sliding window approach is optimal for this problem and has a much-improved time complexity over the brute force approach.

