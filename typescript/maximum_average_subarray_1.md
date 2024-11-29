# [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)

## Approach 1: Brute Force (Basic)

### Solution
typescript
```typescript
// Time Complexity: O(n * k)
// Space Complexity: O(1)
class Solution {
    findMaxAverage(nums: number[], k: number): number {
        let maxAverage = -Infinity;

        // Check all subarrays of size k
        for (let i = 0; i <= nums.length - k; i++) {
            let sum = 0;

            // Calculate the sum of the current subarray
            for (let j = i; j < i + k; j++) {
                sum += nums[j];
            }

            // Update the maximum average
            maxAverage = Math.max(maxAverage, sum / k);
        }

        return maxAverage;
    }
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    findMaxAverage(nums: number[], k: number): number {
        let sum = 0;

        // Calculate the sum of the first window
        for (let i = 0; i < k; i++) {
            sum += nums[i];
        }

        let maxAverage = sum / k;

        // Slide the window over the array
        for (let i = k; i < nums.length; i++) {
            sum += nums[i] - nums[i - k]; // Update the sum for the new window
            maxAverage = Math.max(maxAverage, sum / k); // Update the maximum average
        }

        return maxAverage;
    }
}
```

