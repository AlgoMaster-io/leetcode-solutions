# 198. [House Robber](https://leetcode.com/problems/house-robber/)

## Approach 1: Recursive Solution

### Solution
csharp
```csharp
// Time Complexity: O(2^n)
// Space Complexity: O(n)
public class Solution {
    public int Rob(int[] nums) {
        return RobFrom(0, nums);
    }

    private int RobFrom(int index, int[] nums) {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.Length) {
            return 0;
        }
        
        // Either rob this house and skip one, or skip it
        int robCurrent = nums[index] + RobFrom(index + 2, nums);
        int skipCurrent = RobFrom(index + 1, nums);

        // Return the maximum of both choices
        return Math.Max(robCurrent, skipCurrent);
    }
}
```

## Approach 2: Recursion with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
using System.Collections.Generic;

public class Solution {
    public int Rob(int[] nums) {
        return RobFrom(0, nums, new Dictionary<int, int>());
    }

    private int RobFrom(int index, int[] nums, Dictionary<int, int> memo) {
        // Base case: If index is out of bounds, return 0
        if (index >= nums.Length) {
            return 0;
        }

        // Check memoization table
        if (memo.ContainsKey(index)) {
            return memo[index];
        }

        // Either rob this house and skip one, or skip it
        int robCurrent = nums[index] + RobFrom(index + 2, nums, memo);
        int skipCurrent = RobFrom(index + 1, nums, memo);

        // Memoize the result
        int result = Math.Max(robCurrent, skipCurrent);
        memo[index] = result;

        return result;
    }
}
```

## Approach 3: Dynamic Programming (Bottom Up)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public int Rob(int[] nums) {
        if (nums.Length == 0) return 0;

        int[] dp = new int[nums.Length + 1];
        dp[0] = 0;
        dp[1] = nums[0];

        // Build the dp array
        for (int i = 2; i <= nums.Length; i++) {
            dp[i] = Math.Max(dp[i - 1], dp[i - 2] + nums[i - 1]);
        }

        return dp[nums.Length];
    }
}
```

## Approach 4: Optimized Dynamic Programming (Constant Space)

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int Rob(int[] nums) {
        if (nums.Length == 0) return 0;

        int prev1 = 0; // max money can rob up to the previous house
        int prev2 = 0; // max money can rob up to two houses before

        // Iterate through each house
        foreach (int num in nums) {
            int temp = prev1;
            prev1 = Math.Max(prev2 + num, prev1);
            prev2 = temp;
        }

        return prev1;
    }
}
```

