# [Leetcode 70: Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approaches

1. [Recursive Solution](#recursive-solution)
2. [Dynamic Programming (Top-Down Memoization)](#dynamic-programming-top-down-memoization)
3. [Dynamic Programming (Bottom-Up)](#dynamic-programming-bottom-up)
4. [Optimized Dynamic Programming (Space-Optimization)](#optimized-dynamic-programming-space-optimization)

---

## Recursive Solution

The climbing stairs problem can be solved using a recursive approach. The core idea is to think of climbing stairs as making a series of steps. At each step, you have two choices: take one step or take two steps. 

### Intuition
- Start from the base step (step 0) and determine how many ways can you reach the top step (step `n`).
- At each step `i`, you have two choices: move to `i+1` or `i+2`.
- Recursively calculate the number of ways to reach the top from each of these steps.
- The base cases are:
  - If you reach step `n`, there's 1 way to stay there (do nothing).
  - If you surpass step `n`, there are 0 ways.

### Code

```java
class Solution {
    public int climbStairs(int n) {
        // Recursive helper function
        return climbStairsHelper(n);
    }
    
    private int climbStairsHelper(int n) {
        // Base case: If there are no steps left, there's 1 way (do nothing)
        if (n == 0) return 1;
        // If we surpass the target step, there are no ways
        if (n < 0) return 0;
        
        // Recursive calls for one step and two steps
        return climbStairsHelper(n - 1) + climbStairsHelper(n - 2);
    }
}
```

### Complexity

- **Time Complexity:** O(2^n), because each step splits into two possibilities (recursive calls).
- **Space Complexity:** O(n), due to the recursion stack depth.

---

## Dynamic Programming (Top-Down Memoization)

This approach uses memoization to store results of subproblems, reducing repeated calculations.

### Intuition
- Similar to the recursive approach, but use an array to store already computed results for each step.
- The memoization array `memo` stores results of the number of ways to climb to the top from each step.
- Fill in the memoization table on the fly, storing each result for quick access.

### Code

```java
class Solution {
    public int climbStairs(int n) {
        // Create a memoization array and fill with -1 (indicating untouched steps)
        int[] memo = new int[n + 1];
        return climbStairsMemo(n, memo);
    }
    
    private int climbStairsMemo(int n, int[] memo) {
        if (n == 0) return 1;
        if (n < 0) return 0;
        
        // Return the memoized value if already calculated
        if (memo[n] != -1) return memo[n];
        
        // Calculate the value and store in memo
        memo[n] = climbStairsMemo(n - 1, memo) + climbStairsMemo(n - 2, memo);
        return memo[n];
    }
}
```

### Complexity

- **Time Complexity:** O(n), each step calculation is done once and stored.
- **Space Complexity:** O(n), due to the memoization array.

---

## Dynamic Programming (Bottom-Up)

This approach involves using an iterative dynamic programming approach, filling from the bottom up without recursion.

### Intuition
- Initialize a DP array `dp[]` where `dp[i]` represents the number of ways to reach step `i`.
- Start with base conditions `dp[0] = 1` and `dp[1] = 1`.
- Iteratively fill the array using the relation `dp[i] = dp[i-1] + dp[i-2]`.

### Code

```java
class Solution {
    public int climbStairs(int n) {
        if (n == 1) return 1; // Base case for 1 step
        
        int[] dp = new int[n + 1];
        dp[0] = 1; // No step means one way (do nothing)
        dp[1] = 1; // One step is exactly one step in one way

        // Fill the dp array iteratively
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        
        return dp[n];
    }
}
```

### Complexity

- **Time Complexity:** O(n), each step is calculated once in a loop.
- **Space Complexity:** O(n), stored in the dp array.

---

## Optimized Dynamic Programming (Space-Optimization)

By observing that the DP approach only uses the last two numbers, we can optimize space usage.

### Intuition
- Instead of maintaining the whole DP array, keep only two variables to hold the last two states.
- This reduces space complexity substantially.

### Code

```java
class Solution {
    public int climbStairs(int n) {
        if (n == 1) return 1;
        
        int oneStepBefore = 1;
        int twoStepsBefore = 1;
        int allWays = 0;
        
        for (int i = 2; i <= n; i++) {
            allWays = oneStepBefore + twoStepsBefore; // Current step ways
            twoStepsBefore = oneStepBefore; // Move one step forward
            oneStepBefore = allWays; // Move one step forward
        }
        
        return allWays;
    }
}
```

### Complexity

- **Time Complexity:** O(n), each step is calculated once in a loop.
- **Space Complexity:** O(1), using constant space for last two computed states.

