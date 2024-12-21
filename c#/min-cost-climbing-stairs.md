# [Leetcode 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approaches

- [Approach 1: Recursive Solution with Memoization](#approach-1)
- [Approach 2: Dynamic Programming with Array](#approach-2)
- [Approach 3: Optimized Dynamic Programming with Constant Space](#approach-3)

---

## Approach 1: Recursive Solution with Memoization

### Intuition
The problem can be viewed as a recursive process, where at each step we have the option to climb one or two stairs with an associated cost. To minimize the cost, we select the optimal choice at each step. Since there are overlapping subproblems, we can use memoization to store the cost of climbing from each step to avoid redundant computations.

### Implementation

```csharp
public class Solution {
    public int MinCostClimbingStairs(int[] cost) {
        int n = cost.Length;
        int[] memo = new int[n]; // Array to store minimum costs of reaching the end
        return Math.Min(GetMinCost(cost, n - 1, memo), GetMinCost(cost, n - 2, memo));
    }
    
    private int GetMinCost(int[] cost, int i, int[] memo) {
        if (i < 0) return 0;
        if (memo[i] != 0) return memo[i];
        
        // Calculate the minimum cost for the current step
        int minCost = cost[i] + Math.Min(GetMinCost(cost, i - 1, memo), GetMinCost(cost, i - 2, memo));
        memo[i] = minCost; // Memorizing result

        return minCost;
    }
}
```

### Time Complexity
- **Time Complexity:** O(n) because each element is computed once due to memoization.
- **Space Complexity:** O(n) for the memoization array.

---

## Approach 2: Dynamic Programming with Array

### Intuition
Using dynamic programming, we can build up the solution iteratively. The idea is similar to the recursive approach but using an iterative loop to fill up an auxiliary array that keeps track of the minimum cost to reach each step. By the end, the minimum cost to reach the top of the stairs is the minimum cost between the last step and the second last step.

### Implementation

```csharp
public class Solution {
    public int MinCostClimbingStairs(int[] cost) {
        int n = cost.Length;
        int[] dp = new int[n + 1];
        
        for (int i = 2; i <= n; i++) {
            // Calculate the minimum cost to reach the current step
            dp[i] = Math.Min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }

        return dp[n];
    }
}
```

### Time Complexity
- **Time Complexity:** O(n) for iterating through the cost array.
- **Space Complexity:** O(n) for the dp array.

---

## Approach 3: Optimized Dynamic Programming with Constant Space

### Intuition
The above dynamic programming solution can be space-optimized. Notice that at any step `i`, we only need the values from the two previous steps to compute the current step's cost. Thus, we can reduce the space complexity to O(1) by keeping track of only the last two costs.

### Implementation

```csharp
public class Solution {
    public int MinCostClimbingStairs(int[] cost) {
        int prev1 = 0; // Represents dp[i-1]
        int prev2 = 0; // Represents dp[i-2]
        
        for (int i = 2; i <= cost.Length; i++) {
            int current = Math.Min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
}
```

### Time Complexity
- **Time Complexity:** O(n) because we process each stair step once.
- **Space Complexity:** O(1) for constant space usage.

---

