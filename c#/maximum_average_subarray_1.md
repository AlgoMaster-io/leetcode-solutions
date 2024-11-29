# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n * k)
// Space Complexity: O(1)
public class Solution {
    public double FindMaxAverage(int[] nums, int k) {
        double maxAverage = double.NegativeInfinity;

        // Check all subarrays of size k
        for (int i = 0; i <= nums.Length - k; i++) {
            double sum = 0;

            // Calculate the sum of the current subarray
            for (int j = i; j < i + k; j++) {
                sum += nums[j];
            }

            // Update the maximum average
            maxAverage = Math.Max(maxAverage, sum / k);
        }

        return maxAverage;
    }
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public double FindMaxAverage(int[] nums, int k) {
        double sum = 0;

        // Calculate the sum of the first window
        for (int i = 0; i < k; i++) {
            sum += nums[i];
        }

        double maxAverage = sum / k;

        // Slide the window over the array
        for (int i = k; i < nums.Length; i++) {
            sum += nums[i] - nums[i - k]; // Update the sum for the new window
            maxAverage = Math.Max(maxAverage, sum / k); // Update the maximum average
        }

        return maxAverage;
    }
}
```


