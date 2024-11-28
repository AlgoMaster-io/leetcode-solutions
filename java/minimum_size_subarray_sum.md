# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
```java
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int minLength = Integer.MAX_VALUE;

        // Iterate over all possible starting points
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            // Iterate over all possible ending points
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];

                // Check if the sum is greater than or equal to the target
                if (sum >= target) {
                    minLength = Math.min(minLength, j - i + 1);
                    break; // Break as the subarray length cannot decrease further
                }
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```

## Approach 2: Sliding Window (Optimal)

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int minLength = Integer.MAX_VALUE;

        // Sliding window
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];

            // Shrink the window until the sum is less than the target
            while (sum >= target) {
                minLength = Math.min(minLength, right - left + 1);
                sum -= nums[left];
                left++;
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```