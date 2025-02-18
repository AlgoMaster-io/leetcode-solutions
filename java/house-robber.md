# [Leetcode Problem 198: House Robber](https://leetcode.com/problems/house-robber/)

## Table of Contents
- [Approach 1: Recursive Approach](#approach-1-recursive-approach)
- [Approach 2: Memoization (Top-Down DP)](#approach-2-memoization-top-down-dp)
- [Approach 3: Bottom-Up Dynamic Programming](#approach-3-bottom-up-dynamic-programming)
- [Approach 4: Optimized Space Dynamic Programming](#approach-4-optimized-space-dynamic-programming)


## Approach 1: Recursive Approach

The problem of robbing houses can be thought of as making a choice at each house: either you rob it or you skip it. The base problem is that you cannot rob two consecutive houses. 

We can define a recursive function `robFrom(int i)` which returns the maximum amount of money that can be robbed starting from house `i`. The decision at each house will be:

1. Rob the current house `i` and skip the next house `i+1`.
2. Do not rob the current house `i` and try to rob from house `i+1`.

This will be a naive solution with exponential time complexity, but it establishes the foundation of the problem.

```java
class Solution {
    public int rob(int[] nums) {
        return robFrom(0, nums);
    }
    
    // Recursive function to decide the maximum sum of money
    private int robFrom(int i, int[] nums) {
        // Base case: no more houses to examine
        if (i >= nums.length) return 0;
        
        // Recursive cases
        // Rob current house and move to house after the next
        int rob = nums[i] + robFrom(i + 2, nums);
        // Skip current house and move to the next house
        int skip = robFrom(i + 1, nums);
        
        // Return the maximum of both choices
        return Math.max(rob, skip);
    }
}
```

**Time Complexity**: O(2^n) — In the worst case, where `n` is the number of houses.

**Space Complexity**: O(n) — The depth of the recursion tree can go up to `n`.

## Approach 2: Memoization (Top-Down DP)

To optimize the recursive approach, we can use memoization to store the results of the subproblems that we've already solved, preventing re-calculation and reducing the time complexity from exponential to linear.

```java
class Solution {
    public int rob(int[] nums) {
        int[] memo = new int[nums.length];
        Arrays.fill(memo, -1);
        return robFrom(0, nums, memo);
    }
    
    // Recursive function with memoization
    private int robFrom(int i, int[] nums, int[] memo) {
        // Base condition
        if (i >= nums.length) return 0;
        
        // Return the stored value if the subproblem is already calculated
        if (memo[i] != -1) return memo[i];
        
        // Recursive cases with memoization
        int rob = nums[i] + robFrom(i + 2, nums, memo);
        int skip = robFrom(i + 1, nums, memo);
        
        // Store the result before returning
        memo[i] = Math.max(rob, skip);
        
        return memo[i];
    }
}
```

**Time Complexity**: O(n) — Each house is visited at most once.

**Space Complexity**: O(n) — Due to recursion and memo array.

## Approach 3: Bottom-Up Dynamic Programming

We can reformulate the problem iteratively using a bottom-up dynamic programming approach. We'll systematically determine the best choice at each house leading up to the final decision.

```java
class Solution {
    public int rob(int[] nums) {
        // Edge case: no houses
        if (nums.length == 0) return 0;
        
        int[] dp = new int[nums.length + 1];
        // Base case initialization
        dp[0] = 0;
        dp[1] = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            // Choose to rob the current house or skip it
            dp[i + 1] = Math.max(dp[i], dp[i - 1] + nums[i]);
        }
        
        return dp[nums.length];
    }
}
```

**Time Complexity**: O(n) — Only one pass through the list is needed.

**Space Complexity**: O(n) — For the DP array.

## Approach 4: Optimized Space Dynamic Programming

We can further optimize the space complexity by realizing that at any point, we only need the last two states to make our decision.

```java
class Solution {
    public int rob(int[] nums) {
        // Edge case: no houses
        if (nums.length == 0) return 0;
        
        int prev1 = 0; // dp[i-1]
        int prev2 = 0; // dp[i-2]
        
        for (int num : nums) {
            int temp = prev1;
            // Max of skipping the current house or robbing it 
            prev1 = Math.max(prev1, prev2 + num);
            prev2 = temp;
        }
        
        return prev1;
    }
}
```

**Time Complexity**: O(n) — Once through the list.

**Space Complexity**: O(1) — Constant space usage.

