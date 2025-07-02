# [Leetcode 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## Approaches:
- [Approach 1: Brute Force Recursion](#approach-1-brute-force-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming (Bottom-Up)](#approach-3-dynamic-programming-bottom-up)
- [Approach 4: Optimized Dynamic Programming (Bottom-Up with Constant Space)](#approach-4-optimized-dynamic-programming-bottom-up-with-constant-space)

### Approach 1: Brute Force Recursion
The brute force approach uses recursion to explore all possible ways to reach the top of the stairs. From each step, you can move one step or two steps forward, and we compute the minimum cost recursively.

#### Intuition:
- Starting from step 0 or step 1, at each step, choose to advance either one or two steps.
- Sum the cost of the current step with the minimum cost of reaching the top from the advanced step.
- The base cases are when you reach or exceed the length of the cost array.
  
#### Time and Space Complexity:
- **Time Complexity:** O(2^n) - As we explore all possibilities with repeated calculations.
- **Space Complexity:** O(n) - Space used by the recursion stack.

```python
def minCostClimbingStairs(cost):
    def minCost(i):
        # If we have reached the end or beyond, no cost is needed.
        if i >= len(cost):
            return 0
        # Recursive call for one step ahead and two steps ahead.
        return cost[i] + min(minCost(i + 1), minCost(i + 2))
    
    # You can start from step 0 or step 1.
    return min(minCost(0), minCost(1))
```

### Approach 2: Recursion with Memoization
To avoid redundant calculations of the minimum costs for the same steps over and over, we can use memoization.

#### Intuition:
- Use a dictionary or list to store the already computed results for each step.
- Whenever you calculate the cost for a step, store it in a dictionary.
- Before computing the cost for a step, check if it is already in the dictionary.

#### Time and Space Complexity:
- **Time Complexity:** O(n) - Each step is computed once.
- **Space Complexity:** O(n) - Space used for the memoization dictionary.

```python
def minCostClimbingStairs(cost):
    memo = {}
    
    def minCost(i):
        if i >= len(cost):
            return 0
        if i in memo:
            return memo[i]
        memo[i] = cost[i] + min(minCost(i + 1), minCost(i + 2))
        return memo[i]
    
    return min(minCost(0), minCost(1))
```

### Approach 3: Dynamic Programming (Bottom-Up)
Using dynamic programming, we fill an array with the minimum costs from the base to the top step.

#### Intuition:
- Create an array `dp` where `dp[i]` holds the cost to reach the top from step `i`.
- Base cases: cost to climb beyond the top is zero.
- Fill the `dp` array from the end of the cost array to the beginning.

#### Time and Space Complexity:
- **Time Complexity:** O(n) - We traverse the array once.
- **Space Complexity:** O(n) - We store results for each step in the `dp` array.

```python
def minCostClimbingStairs(cost):
    n = len(cost)
    dp = [0] * (n + 2)
    
    # Start calculating the minimum cost from the top to the base.
    for i in range(n - 1, -1, -1):
        dp[i] = cost[i] + min(dp[i + 1], dp[i + 2])
    
    # We can start from the first step or the second step.
    return min(dp[0], dp[1])
```

### Approach 4: Optimized Dynamic Programming (Bottom-Up with Constant Space)
Instead of maintaining an array, keep only the last two computations as they are enough to compute the current one.

#### Intuition:
- Maintain two variables to store the minimum costs for the two steps directly above the current step.
- Update variables as you progress downwards from the last step.

#### Time and Space Complexity:
- **Time Complexity:** O(n) - We traverse the array once.
- **Space Complexity:** O(1) - Constant space for calculations.

```python
def minCostClimbingStairs(cost):
    n = len(cost)
    if n == 0: return 0
    if n == 1: return cost[0]

    # Initialize the costs for one step and two steps from the top.
    next1, next2 = 0, 0
    
    for i in range(n - 1, -1, -1):
        current = cost[i] + min(next1, next2)
        next1, next2 = current, next1
    
    return min(next1, next2)
```

These approaches illustrate the progression from inefficient to efficient solutions using dynamic programming techniques for the problem of finding the minimum cost to climb stairs.

