# 746. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    minCostClimbingStairs(cost: number[]): number {
        // Create a memoization array for storing results of subproblems
        const memo: number[] = new Array(cost.length).fill(0);
        return Math.min(this.minCost(cost.length - 1, cost, memo),
                        this.minCost(cost.length - 2, cost, memo));
    }

    private minCost(i: number, cost: number[], memo: number[]): number {
        if (i < 0) return 0; // Base case: No cost for negative steps
        if (memo[i] !== 0) return memo[i]; // Return precomputed cost if available

        // Minimum cost to reach this step
        const costToReach = cost[i] + Math.min(this.minCost(i - 1, cost, memo), 
                                              this.minCost(i - 2, cost, memo));
        
        memo[i] = costToReach; // Store computed result in memo array
        return costToReach;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    minCostClimbingStairs(cost: number[]): number {
        const n = cost.length;
        const dp: number[] = new Array(n + 1).fill(0);
        
        // Base cases: No cost to start at either step 0 or step 1
        dp[0] = 0;
        dp[1] = 0;

        // Fill the dp array with minimum cost to reach each step
        for (let i = 2; i <= n; i++) {
            dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }

        return dp[n]; // Minimum cost to reach the top
    }
}
```

## Approach 3: Dynamic Programming with Constant Space

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    minCostClimbingStairs(cost: number[]): number {
        const n = cost.length;
        let prev1 = 0;
        let prev2 = 0;

        // Iterate through cost array and calculate minimum cost dynamically
        for (let i = 2; i <= n; i++) {
            const current = Math.min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
            prev2 = prev1;
            prev1 = current;
        }

        return prev1; // Minimum cost to reach the top
    }
}
```

