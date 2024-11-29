# 494. [Target Sum](https://leetcode.com/problems/target-sum/)

## Approach 1: Recursive Brute Force

### Solution
```java
// Time Complexity: O(2^n)
// Space Complexity: O(n)
public class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        return calculate(nums, 0, 0, target);
    }
    
    private int calculate(int[] nums, int i, int sum, int target) {
        // Base case: when we've considered all numbers
        if (i == nums.length) {
            // Return 1 if we've reached the target sum, 0 otherwise
            return sum == target ? 1 : 0;
        }
    
        // Explore both adding and subtracting the current number
        return calculate(nums, i + 1, sum + nums[i], target) 
             + calculate(nums, i + 1, sum - nums[i], target);
    }
}
```

## Approach 2: Dynamic Programming with Memoization

### Solution
```java
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
import java.util.HashMap;

public class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        // Use HashMap to store calculated results for memoization
        return calculate(nums, 0, 0, target, new HashMap<>());
    }
    
    private int calculate(int[] nums, int i, int sum, int target, HashMap<String, Integer> memo) {
        String key = i + "," + sum;
        
        // If result is already calculated for this state, return it
        if (memo.containsKey(key)) {
            return memo.get(key);
        }
        
        // Base case: when we've considered all numbers
        if (i == nums.length) {
            return sum == target ? 1 : 0;
        }
        
        // Explore both adding and subtracting the current number
        int add = calculate(nums, i + 1, sum + nums[i], target, memo);
        int subtract = calculate(nums, i + 1, sum - nums[i], target, memo);
        
        // Save result in memo
        memo.put(key, add + subtract);
        return add + subtract;
    }
}
```

## Approach 3: Dynamic Programming with Bottom-Up

### Solution
```java
// Time Complexity: O(n * target)
// Space Complexity: O(n * target)
public class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        
        // Calculate total sum of the array
        for (int num : nums) {
            sum += num;
        }
        
        // If sum is smaller than target or (sum + target) is odd, return 0
        if (Math.abs(target) > sum || (sum + target) % 2 != 0) {
            return 0;
        }
        
        int s = (sum + target) / 2;
        
        // dp[i] represents the number of ways to get the sum i
        int[] dp = new int[s + 1];
        dp[0] = 1;
        
        // Update dp array
        for (int num : nums) {
            for (int j = s; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }
        
        return dp[s];
    }
}
```



