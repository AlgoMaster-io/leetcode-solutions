# [Leetcode 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approaches
1. [Recursive Approach with Memoization](#recursive-approach-with-memoization)
2. [Dynamic Programming with Bottom-Up Approach](#dynamic-programming-with-bottom-up-approach)
3. [Dynamic Programming with Constant Space Optimization](#dynamic-programming-with-constant-space-optimization)

## Recursive Approach with Memoization

### Intuition
The problem can be visualized as a decision-making problem at each step. You can choose to take either one step or two and incur a cost. A natural way to solve this problem is through recursion by trying all possible paths and storing the results of subproblems to avoid recomputation. This will be done using memoization.

### Code
```typescript
function minCostClimbingStairs(cost: number[]): number {
    const n = cost.length;
    const memo: number[] = new Array(n).fill(-1);
    
    // Helper function for recursion with memoization
    function minCost(i: number): number {
        // Base case: if step is beyond the last step, no cost needed
        if (i >= n) return 0;
        
        // If already computed, return the stored result
        if (memo[i] !== -1) return memo[i];
        
        // Recursive case: choose either one step or two
        memo[i] = cost[i] + Math.min(minCost(i + 1), minCost(i + 2));
        return memo[i];
    }
    
    // Start from step 0 or step 1
    return Math.min(minCost(0), minCost(1));
}
```

### Complexity Analysis
- **Time Complexity:** O(n) - Each step's cost is computed once and stored.
- **Space Complexity:** O(n) - The space is used by the memoization array.

## Dynamic Programming with Bottom-Up Approach

### Intuition
This approach builds from the base cases upwards. It populates an array with the minimum cost for reaching each step, utilizing the information from previous steps. This eliminates the need for recursion.

### Code
```typescript
function minCostClimbingStairs(cost: number[]): number {
    const n = cost.length;
    const dp: number[] = new Array(n + 1).fill(0);
    
    // Base cases
    dp[0] = 0; 
    dp[1] = 0;
    
    // Fill the dp array with minimum cost to reach each step
    for (let i = 2; i <= n; i++) {
        dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
    }
    
    // Minimum cost to reach the top can be from last or second-last step
    return dp[n];
}
```

### Complexity Analysis
- **Time Complexity:** O(n) - The loop runs over the cost array once.
- **Space Complexity:** O(n) - Additional space is used by the dp array.

## Dynamic Programming with Constant Space Optimization

### Intuition
Instead of storing results for all steps, we realize only the last two computations are needed to calculate the current one. Thus, we optimize space by storing only two variables to hold these values.

### Code
```typescript
function minCostClimbingStairs(cost: number[]): number {
    const n = cost.length;
    let prev2 = 0;  // Min cost to reach step i-2
    let prev1 = 0;  // Min cost to reach step i-1
    
    // Iterate through the array to find the minimum cost to reach each step
    for (let i = 2; i <= n; i++) {
        const current = Math.min(prev1 + cost[i - 1], prev2 + cost[i - 2]);
        prev2 = prev1; // Update prev2 to the last step
        prev1 = current; // Update prev1 to the current step
    }
    
    // The minimum cost to reach the top
    return prev1;
}
```

### Complexity Analysis
- **Time Complexity:** O(n) - We iterate over the cost array once.
- **Space Complexity:** O(1) - Only a constant amount of extra space is used.

Each approach has its own use case depending on constraints and space requirements, with the third approach being the most space-efficient.

