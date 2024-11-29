# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Approach 1: Brute Force (Basic)

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(1)
public class Solution {
    public int MinSubArrayLen(int target, int[] nums) {
        int minLength = int.MaxValue;

        // Iterate over all possible starting points
        for (int i = 0; i < nums.Length; i++) {
            int sum = 0;

            // Iterate over all possible ending points
            for (int j = i; j < nums.Length; j++) {
                sum += nums[j];

                // Check if the sum is greater than or equal to the target
                if (sum >= target) {
                    minLength = Math.Min(minLength, j - i + 1);
                    break; // Break as the subarray length cannot decrease further
                }
            }
        }

        return minLength == int.MaxValue ? 0 : minLength;
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
    public int MinSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int minLength = int.MaxValue;

        // Sliding window
        for (int right = 0; right < nums.Length; right++) {
            sum += nums[right];

            // Shrink the window until the sum is less than the target
            while (sum >= target) {
                minLength = Math.Min(minLength, right - left + 1);
                sum -= nums[left];
                left++;
            }
        }

        return minLength == int.MaxValue ? 0 : minLength;
    }
}
```

