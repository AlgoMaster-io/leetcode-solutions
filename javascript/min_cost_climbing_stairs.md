# 746. [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function minCostClimbingStairs(cost) {
    const memo = new Array(cost.length).fill(0);
    return Math.min(minCost(cost.length - 1, cost, memo),
                    minCost(cost.length - 2, cost, memo));
}

function minCost(i, cost, memo) {
    if (i < 0) return 0; // Base case: No cost for negative steps
    if (memo[i] !== 0) return memo[i]; // Return precomputed cost if available

    // Minimum cost to reach this step
    const costToReach = cost[i] + Math.min(minCost(i - 1, cost, memo), 
                                           minCost(i - 2, cost, memo));
    
    memo[i] = costToReach; // Store computed result in memo array
    return costToReach;
}
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function minCostClimbingStairs(cost) {
    const n = cost.length;
    const dp = new Array(n + 1).fill(0);
    
    // Base cases: No cost to start at either step 0 or step 1
    dp[0] = 0;
    dp[1] = 0;

    // Fill the dp array with minimum cost to reach each step
    for (let i = 2; i <= n; i++) {
        dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
    }

    return dp[n]; // Minimum cost to reach the top
}
```

## Approach 3: Dynamic Programming with Constant Space

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function minCostClimbingStairs(cost) {
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
```

