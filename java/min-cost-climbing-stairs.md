
[Leetcode Problem 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

### Approaches:
1. [Recursion](#recursion)
2. [Top-Down Dynamic Programming (Memoization)](#top-down-dynamic-programming-memoization)
3. [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)
4. [Optimized Bottom-Up Approach](#optimized-bottom-up-approach)

---

## Recursion

### Intuition:
Start by identifying that you can reach step `i` from either step `i-1` or `i-2`. This gives rise to a recursive relation where the cost to reach the `i-th` step is the cost of the step itself plus the minimum of the costs to reach the two preceding steps.

### Code:
```java
public class Solution {
    public int minCostClimbingStairs(int[] cost) {
        return Math.min(minCost(cost, cost.length - 1), minCost(cost, cost.length - 2));
    }
    
    private int minCost(int[] cost, int i) {
        if (i < 0) {
            return 0;
        }
        if (i == 0 || i == 1) {
            return cost[i];
        }
        // Calculate the minimum cost to reach step i
        return cost[i] + Math.min(minCost(cost, i - 1), minCost(cost, i - 2));
    }
}
```

### Time Complexity:
O(2^n) - The function calculates each step repeatedly.

### Space Complexity:
O(n) - Due to stack space used in recursion.

---

## Top-Down Dynamic Programming (Memoization)

### Intuition:
To optimize the recursive solution, we can store the results of subproblems (i.e., costs for each step) and reuse them instead of solving the same subproblems repeatedly.

### Code:
```java
public class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] memo = new int[cost.length];
        return Math.min(minCost(cost, cost.length - 1, memo), minCost(cost, cost.length - 2, memo));
    }
    
    private int minCost(int[] cost, int i, int[] memo) {
        if (i < 0) {
            return 0;
        }
        if (i == 0 || i == 1) {
            return cost[i];
        }
        if (memo[i] == 0) {
            // Compute the minimum cost for step i if not done yet
            memo[i] = cost[i] + Math.min(minCost(cost, i - 1, memo), minCost(cost, i - 2, memo));
        }
        // Return the stored result
        return memo[i];
    }
}
```

### Time Complexity:
O(n) - Each step's cost is computed only once.

### Space Complexity:
O(n) - Due to place for memoization.

---

## Bottom-Up Dynamic Programming

### Intuition:
Iteratively build up the solution from the base cases (steps 0 and 1) to the target steps, avoiding recursion overhead.

### Code:
```java
public class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        if (n == 0) return 0;
        if (n == 1) return cost[0];
        
        int[] dp = new int[n];
        dp[0] = cost[0];
        dp[1] = cost[1];
        
        for (int i = 2; i < n; i++) {
            // Calculate the minimum cost to reach step i
            dp[i] = cost[i] + Math.min(dp[i - 1], dp[i - 2]);
        }
        // Cost to get past the last two steps to the top
        return Math.min(dp[n - 1], dp[n - 2]);
    }
}
```

### Time Complexity:
O(n) - We only iterate through the array once.

### Space Complexity:
O(n) - To store minimal costs for each step.

---

## Optimized Bottom-Up Approach

### Intuition:
Instead of maintaining an array for dynamic programming, we only need to keep track of the last two costs since the cost to reach each step depends only on the two preceding steps.

### Code:
```java
public class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int n = cost.length;
        if (n == 0) return 0;
        if (n == 1) return cost[0];
        
        int first = cost[0];
        int second = cost[1];
        
        for (int i = 2; i < n; i++) {
            // Calculate the new minimum cost using the last two values
            int current = cost[i] + Math.min(first, second);
            first = second;
            second = current;
        }
        // Cost to get past the last two steps
        return Math.min(first, second);
    }
}
```

### Time Complexity:
O(n) - Only a single pass through the input is needed.

### Space Complexity:
O(1) - Use only a fixed amount of extra space, independent of input size.

