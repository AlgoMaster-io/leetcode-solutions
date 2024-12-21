# [Leetcode 198: House Robber](https://leetcode.com/problems/house-robber/)

## Approaches
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming (Bottom-up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Optimized Dynamic Programming with Constant Space](#approach-4-optimized-dynamic-programming-with-constant-space)

### Approach 1: Recursive Solution

**Intuition**:
- The robber has two choices at each house: rob it or skip it. If the robber chooses to rob the current house, they must skip the next house. Otherwise, they can consider robbing the next house.
- This leads to a recursion where for each house, the robber decides whether to include it in the sum or not.

```csharp
public class Solution {
    public int Rob(int[] nums) {
        return RobHouse(nums, nums.Length - 1);
    }
    
    private int RobHouse(int[] nums, int i) {
        if (i < 0) return 0; // No more houses to rob
        // Max of robbing current house and skipping one, or skipping current house
        return Math.Max(RobHouse(nums, i - 2) + nums[i], RobHouse(nums, i - 1));
    }
}
```

**Complexity**:
- Time: \(O(2^n)\) - Exponential, as each house leads to two recursive calls.
- Space: \(O(n)\) - Space used on the call stack.

### Approach 2: Recursion with Memoization

**Intuition**:
- A lot of subproblems in the recursive solution are recalculated multiple times. We can use an array to store results of these subproblems to avoid redundant calculations.

```csharp
public class Solution {
    public int Rob(int[] nums) {
        int[] memo = new int[nums.Length];
        Array.Fill(memo, -1);
        return RobHouse(nums, nums.Length - 1, memo);
    }
    
    private int RobHouse(int[] nums, int i, int[] memo) {
        if (i < 0) return 0; // No more houses to rob
        if (memo[i] >= 0) return memo[i]; // Already computed
        
        // Compute and store the result
        memo[i] = Math.Max(RobHouse(nums, i - 2, memo) + nums[i], RobHouse(nums, i - 1, memo));
        return memo[i];
    }
}
```

**Complexity**:
- Time: \(O(n)\) - Each house is computed exactly once.
- Space: \(O(n)\) - For memoization array and recursion stack.

### Approach 3: Dynamic Programming (Bottom-up)

**Intuition**:
- Instead of starting from the last house, we can iterate from the first house and compute the maximum profit at each step using the previously computed values.

```csharp
public class Solution {
    public int Rob(int[] nums) {
        if (nums.Length == 0) return 0;
        if (nums.Length == 1) return nums[0];
        
        int[] dp = new int[nums.Length];
        dp[0] = nums[0];
        dp[1] = Math.Max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.Length; i++) {
            dp[i] = Math.Max(dp[i - 1], dp[i - 2] + nums[i]); // Choose max between rob or skip
        }
        
        return dp[nums.Length - 1];
    }
}
```

**Complexity**:
- Time: \(O(n)\) - Linear pass through the array.
- Space: \(O(n)\) - Array to store results for each house.

### Approach 4: Optimized Dynamic Programming with Constant Space

**Intuition**:
- Instead of storing the results for all houses, we only need the last two results to compute the current result, allowing us to reduce space usage.

```csharp
public class Solution {
    public int Rob(int[] nums) {
        if (nums.Length == 0) return 0;
        if (nums.Length == 1) return nums[0];
        
        int prev1 = 0; // dp[i-1]
        int prev2 = 0; // dp[i-2]
        
        for (int i = 0; i < nums.Length; i++) {
            // Temporary variable to store new dp[i]
            int temp = Math.Max(prev1, prev2 + nums[i]);
            prev2 = prev1; // Update dp[i-2] to dp[i-1]
            prev1 = temp; // Update dp[i-1] to current dp[i]
        }
        
        return prev1; // As prev1 represents dp[n] at the end
    }
}
```

**Complexity**:
- Time: \(O(n)\) - Linear pass through the array.
- Space: \(O(1)\) - Only storing two previous results.

Each approach progressively optimizes on either time or space complexity, leveraging recursion and dynamic programming techniques.

