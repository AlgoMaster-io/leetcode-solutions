[Climbing Stairs - LeetCode Problem](https://leetcode.com/problems/climbing-stairs/)

- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)
- [Approach 4: Fibonacci Optimization](#approach-4-fibonacci-optimization)

## Approach 1: Recursion

### Intuition
The problem can be broken down into a classic recursive structure. At each step, you can either take one step or two steps. Hence, to get to a step `n`, you can be either at step `n-1` or at step `n-2`. This means that the total ways to get to step `n` is the sum of ways to get to `n-1` and `n-2`.

### Recursive Solution
```csharp
public class Solution {
    public int ClimbStairs(int n) {
        // Base cases
        if (n == 1) return 1; // Only 1 way to reach the first stair
        if (n == 2) return 2; // Two ways: (1+1) or (2)

        // Recursive call for previous two steps
        return ClimbStairs(n - 1) + ClimbStairs(n - 2);
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(2^n). This solution is highly inefficient because it recalculates the same values multiple times.
- **Space Complexity**: O(n). The maximum depth of the recursion tree is `n`.

## Approach 2: Recursion with Memoization

### Intuition
To improve upon the naive recursive solution, we can use memoization to store results of subproblems to avoid redundant calculations.

### Code with Memoization
```csharp
public class Solution {
    public int ClimbStairs(int n) {
        int[] memo = new int[n + 1];
        return ClimbStairsMemo(n, memo);
    }

    private int ClimbStairsMemo(int n, int[] memo) {
        if (n == 1) return 1;
        if (n == 2) return 2;
        
        // Check the memo array if value is already computed
        if (memo[n] == 0) {
            memo[n] = ClimbStairsMemo(n - 1, memo) + ClimbStairsMemo(n - 2, memo);
        }
        
        return memo[n];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n). Each subproblem is computed once.
- **Space Complexity**: O(n). For the memo array and the recursion call stack.

## Approach 3: Dynamic Programming

### Intuition
Instead of using recursion, we can iteratively build the solution using a bottom-up dynamic programming approach. We define an array `dp` where `dp[i]` will be the number of ways to reach step `i`.

### DP Solution
```csharp
public class Solution {
    public int ClimbStairs(int n) {
        if (n == 1) return 1;

        int[] dp = new int[n + 1];
        dp[1] = 1;
        dp[2] = 2;

        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
            // dp[i] is calculated as the sum of the ways to step i-1 and i-2
        }

        return dp[n];
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n). Just a single iteration from 3 to n.
- **Space Complexity**: O(n). Additional space for the `dp` array.

## Approach 4: Fibonacci Optimization

### Intuition
Notice how the solution is based on the Fibonacci sequence. Instead of maintaining an array, we can keep only the last two results, thus optimizing space usage to constant space.

### Optimized Fibonacci Solution
```csharp
public class Solution {
    public int ClimbStairs(int n) {
        if (n == 1) return 1;

        int first = 1, second = 2; // First: # of ways to reach step 1, Second: # of ways to reach step 2

        for (int i = 3; i <= n; i++) {
            int third = first + second; // # of ways to reach step i
            first = second; // Move forward
            second = third;
        }

        return second;
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n). Same as previous.
- **Space Complexity**: O(1). Only a few variables for computations.

