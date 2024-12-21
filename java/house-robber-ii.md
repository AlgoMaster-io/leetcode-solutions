# [House Robber II](https://leetcode.com/problems/house-robber-ii/)

### Solutions
- [Approach 1: Recursive Solution with Memoization](#approach-1-recursive-solution-with-memoization)
- [Approach 2: Dynamic Programming with Optimized Space](#approach-2-dynamic-programming-with-optimized-space)

## Approach 1: Recursive Solution with Memoization

### Intuition

In this problem, houses are arranged in a circle. This means the first house is a neighbor of the last house. To avoid robbing adjacent houses, the problem can be broken down into two subproblems:

1. Rob houses from index `0` to `n-2` (excluding the last house).
2. Rob houses from index `1` to `n-1` (excluding the first house).

Use a helper function to recursively calculate the maximum money that can be robbed, while using memoization to avoid redundant calculations.

### Solution

```java
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        return Math.max(robRange(nums, 0, nums.length - 2), robRange(nums, 1, nums.length - 1));
    }

    private int robRange(int[] nums, int start, int end) {
        int n = end - start + 1;
        int[] memo = new int[n];
        return robHelper(nums, start, end, memo);
    }

    private int robHelper(int[] nums, int start, int end, int[] memo) {
        if (start > end) return 0;
        if (memo[start] != 0) return memo[start];
        // Rob this house or skip to next one
        int rob = nums[start] + robHelper(nums, start + 2, end, memo);
        int skip = robHelper(nums, start + 1, end, memo);
        // Store the max value calculated
        memo[start] = Math.max(rob, skip);
        return memo[start];
    }
}
```

### Time Complexity

- O(n), where n is the length of the array because each house is calculated once.

### Space Complexity

- O(n), due to the space used for the memoization array.

## Approach 2: Dynamic Programming with Optimized Space

### Intuition

This approach uses the same two subproblems as the previous method. However, we optimize the space complexity by storing only the last two results (as we only need these to calculate the current house's decision).

### Solution

```java
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) return nums[0];
        return Math.max(robRange(nums, 0, nums.length - 2), robRange(nums, 1, nums.length - 1));
    }

    private int robRange(int[] nums, int start, int end) {
        int prev1 = 0; // Store the result of i-1
        int prev2 = 0; // Store the result of i-2
        for (int i = start; i <= end; i++) {
            int temp = Math.max(prev1, prev2 + nums[i]);
            // Move previous results one step forward
            prev2 = prev1;
            prev1 = temp;
        }
        return prev1;
    }
}
```

### Time Complexity

- O(n), linear traversal of array.

### Space Complexity

- O(1), space usage is constant due to only using fixed variables for calculations.

