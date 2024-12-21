# [Leetcode 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Top-Down Dynamic Programming with Memoization](#top-down-dynamic-programming-with-memoization)
3. [Bottom-Up Dynamic Programming](#bottom-up-dynamic-programming)
4. [Optimized Bottom-Up Dynamic Programming](#optimized-bottom-up-dynamic-programming)

---

## Recursive Approach

### Intuition

The problem requires finding the minimum cost to reach the top of the stairs, starting from either step 0 or 1. We can think of this as making a choice at each step — either taking a single step or two steps forward. By using recursion, we can evaluate the cost of taking either path and choose the one with the minimum cost.

### Code

```javascript
function minCostClimbingStairs(cost) {
    function minCost(i) {
        // Base case: if we are on the first or second step, no cost is required to stand on it
        if (i <= 1) return 0;

        // Calculate the minimum cost to reach the current step from the two previous steps
        return Math.min(minCost(i - 1) + cost[i - 1], minCost(i - 2) + cost[i - 2]);
    }

    // We calculate the minimum cost starting from the top of the stairs and work backwards
    return minCost(cost.length);
}
```

### Time and Space Complexity

- Time Complexity: \(O(2^n)\) - This is because we are exploring all possible combinations of steps.
- Space Complexity: \(O(n)\) - Due to the recursion stack.

---

## Top-Down Dynamic Programming with Memoization

### Intuition

The recursive approach does redundant calculations. To optimize it, we can use memoization to store the result of subproblems, ensuring that each subproblem is solved only once.

### Code

```javascript
function minCostClimbingStairs(cost) {
    const memo = new Map();

    function minCost(i) {
        // Return the saved result if it exists
        if (memo.has(i)) return memo.get(i);
        // Base case: if we are on the first or second step, no cost is required to stand on it
        if (i <= 1) return 0;

        // Calculate the result and store it in the memo dictionary
        const result = Math.min(minCost(i - 1) + cost[i - 1], minCost(i - 2) + cost[i - 2]);
        memo.set(i, result);
        
        return result;
    }

    // We calculate the minimum cost starting from the top of the stairs and work backwards
    return minCost(cost.length);
}
```
 
### Time and Space Complexity

- Time Complexity: \(O(n)\) - Each step is solved once.
- Space Complexity: \(O(n)\) - For the memoization map.

---

## Bottom-Up Dynamic Programming

### Intuition

Instead of top-down, we can build our solution from the ground up. We iteratively compute the minimum costs starting from the base cases and build up to the solution.

### Code

```javascript
function minCostClimbingStairs(cost) {
    const n = cost.length;
    const dp = new Array(n + 1).fill(0);

    // Fill dp array with the minimum cost for reaching each step
    for (let i = 2; i <= n; i++) {
        dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
    }

    // Return the minimum cost to reach the top of the staircase
    return dp[n];
}
```

### Time and Space Complexity

- Time Complexity: \(O(n)\) - We iterate over the steps once.
- Space Complexity: \(O(n)\) - For the dp array.

---

## Optimized Bottom-Up Dynamic Programming

### Intuition

We notice that we only need the last two computed costs to determine the cost of the current step. This means we can optimize our space usage by maintaining only two variables.

### Code

```javascript
function minCostClimbingStairs(cost) {
    const n = cost.length;
    
    let first = 0; // Min cost to reach one step before current
    let second = 0; // Min cost to reach two steps before current

    // Iterate over the steps
    for (let i = 2; i <= n; i++) {
        const current = Math.min(first + cost[i - 1], second + cost[i - 2]);
        second = first; // Update second (two steps before)
        first = current; // Update first (one step before)
    }

    // Return the minimum cost to reach the top of the staircase
    return first;
}
```

### Time and Space Complexity

- Time Complexity: \(O(n)\) - We iterate over the steps once.
- Space Complexity: \(O(1)\) - We use only a constant amount of space.

