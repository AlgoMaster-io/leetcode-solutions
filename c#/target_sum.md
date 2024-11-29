# 494. [Target Sum](https://leetcode.com/problems/target-sum/)

## Approach 1: Recursive Brute Force

### Solution
csharp
```csharp
// Time Complexity: O(2^n)
// Space Complexity: O(n)
public class Solution {
    public int FindTargetSumWays(int[] nums, int target) {
        return Calculate(nums, 0, 0, target);
    }
    
    private int Calculate(int[] nums, int i, int sum, int target) {
        // Base case: when we've considered all numbers
        if (i == nums.Length) {
            // Return 1 if we've reached the target sum, 0 otherwise
            return sum == target ? 1 : 0;
        }
    
        // Explore both adding and subtracting the current number
        return Calculate(nums, i + 1, sum + nums[i], target) 
             + Calculate(nums, i + 1, sum - nums[i], target);
    }
}
```

## Approach 2: Dynamic Programming with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
using System.Collections.Generic;

public class Solution {
    public int FindTargetSumWays(int[] nums, int target) {
        // Use Dictionary to store calculated results for memoization
        return Calculate(nums, 0, 0, target, new Dictionary<string, int>());
    }
    
    private int Calculate(int[] nums, int i, int sum, int target, Dictionary<string, int> memo) {
        string key = i + "," + sum;
        
        // If result is already calculated for this state, return it
        if (memo.ContainsKey(key)) {
            return memo[key];
        }
        
        // Base case: when we've considered all numbers
        if (i == nums.Length) {
            return sum == target ? 1 : 0;
        }
        
        // Explore both adding and subtracting the current number
        int add = Calculate(nums, i + 1, sum + nums[i], target, memo);
        int subtract = Calculate(nums, i + 1, sum - nums[i], target, memo);
        
        // Save result in memo
        memo[key] = add + subtract;
        return add + subtract;
    }
}
```

## Approach 3: Dynamic Programming with Bottom-Up

### Solution
csharp
```csharp
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
public class Solution {
    public int FindTargetSumWays(int[] nums, int target) {
        int sum = 0;
        
        // Calculate total sum of the array
        foreach (int num in nums) {
            sum += num;
        }
        
        // If sum is smaller than target or (sum + target) is odd, return 0
        if (Math.Abs(target) > sum || (sum + target) % 2 != 0) {
            return 0;
        }
        
        int s = (sum + target) / 2;
        
        // dp[i] represents the number of ways to get the sum i
        int[] dp = new int[s + 1];
        dp[0] = 1;
        
        // Update dp array
        foreach (int num in nums) {
            for (int j = s; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        
        return dp[s];
    }
}
```

