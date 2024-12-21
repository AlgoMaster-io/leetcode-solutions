# [Leetcode 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approaches
- [Approach 1: Recursion with Memoization](#approach-1-recursion-with-memoization)
- [Approach 2: Iterative Dynamic Programming](#approach-2-iterative-dynamic-programming)
- [Approach 3: Optimized Dynamic Programming](#approach-3-optimized-dynamic-programming)

## Approach 1: Recursion with Memoization

### Intuition
The problem is essentially about finding the minimal path to reach the top of the stairs. We can consider the problem as a decision at each step to either take 1 step or 2 steps. Analyzing this via recursion could result in overlapping subproblems, making it a good candidate for memoization. 

### Algorithm
The recursion considers two cases:
1. Take a single step from the current position.
2. Take two steps from the current position.

We compute the minimum cost from both cases recursively and store results using memoization to avoid recalculating the results of overlapping subproblems.

### Code
```go
func minCostClimbingStairs(cost []int) int {
    memo := make(map[int]int)
    return min(dfs(cost, len(cost)-1, memo), dfs(cost, len(cost)-2, memo))
}

func dfs(cost []int, i int, memo map[int]int) int {
    if i < 0 {
        return 0
    }
    if i == 0 || i == 1 {
        return cost[i]
    }
    if val, ok := memo[i]; ok {
        return val
    }
    memo[i] = cost[i] + min(dfs(cost, i-1, memo), dfs(cost, i-2, memo))
    return memo[i]
}

func min(a, b int) int {
    if a < b { 
        return a 
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(n), where n is the number of steps. Each step is computed once.
- **Space Complexity**: O(n), for the recursion stack and memoization.

## Approach 2: Iterative Dynamic Programming

### Intuition
Converting the recursive approach into an iterative one obeys the same base cases but eliminates the recursion stack overhead. We iteratively compute the minimum cost using dynamic programming in a bottom-up manner.

### Algorithm
- Compute minimum cost iteratively.
- Use a dp array where dp[i] represents the minimum cost to reach step i.
- Compute dp[i] as the minimum of stepping from i-1 or i-2.

### Code
```go
func minCostClimbingStairs(cost []int) int {
    n := len(cost)
    dp := make([]int, n+1)
    
    dp[0], dp[1] = 0, 0 // Since we can start from step 0 or step 1

    for i := 2; i <= n; i++ {
        dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])
    }

    return dp[n]
}

func min(a, b int) int {
    if a < b { 
        return a 
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(n), as we are iterating through the steps once.
- **Space Complexity**: O(n), for the dp array.

## Approach 3: Optimized Dynamic Programming

### Intuition
The iterative DP solution uses an array `dp` to store computations for step transits, which is not memory efficient as we only need values from the last two calculations. Thus, we can simplify to a constant space solution by keeping track of only the last two steps.

### Algorithm
- Instead of a full length array, maintain only two variables to represent the costs of the last two positions.
- Compute the current step using these two and update them for the next iteration.

### Code
```go
func minCostClimbingStairs(cost []int) int {
    prev, current := 0, 0

    for i := 2; i <= len(cost); i++ {
        next := min(current + cost[i-1], prev + cost[i-2])
        prev, current = current, next
    }

    return current
}

func min(a, b int) int {
    if a < b { 
        return a 
    }
    return b
}
```

### Complexity
- **Time Complexity**: O(n), a single loop through the steps.
- **Space Complexity**: O(1), as we are only maintaining two variables.

Each approach provides a progressively optimized solution from recursion with memoization to the final iterative approach with constant space usage.

