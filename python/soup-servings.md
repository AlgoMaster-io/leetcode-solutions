## [Leetcode 808: Soup Servings](https://leetcode.com/problems/soup-servings/)

### Approaches:
1. [Recursive Dynamic Programming (Top-Down Approach)](#approach-1-recursive-dynamic-programming)
2. [Iterative Dynamic Programming (Bottom-Up Approach)](#approach-2-iterative-dynamic-programming)

---

### Approach 1: Recursive Dynamic Programming

The problem deals with serving two different quantities of soup, A and B, and the goal is to find the probability that soup A gets depleted first.

#### Intuition:
- We can express the problem as a recursive function where we consider all possible ways of serving soup A and soup B with different amounts.
- Since there are only a few distinct operations that deplete a small portion of each soup, we use these as our base operations.
- To avoid recalculating probabilities for the same amounts of soup repeatedly, we use memoization.

#### Detailed Steps:
1. Define a function `dfs(a, b)` representing the probability that soup A will be depleted first given `a` units of soup A and `b` units of soup B.
2. Consider the probability of each type of operation that reduces portions of soup A and B. The operations are (100, 0), (75, 25), (50, 50), and (25, 75).
3. Use memoization to store already computed results for specific (a, b) pairs.
4. Base Cases:
   - If `a <= 0` and `b <= 0`, both are depleted, return 0.5 (probability of being finished at the same time).
   - If `a <= 0`, A is depleted first, return 1.
   - If `b <= 0`, B is depleted first, return 0.
5. We will solve by scaling down the problem, using recursion to handle smaller and smaller servings until the result is reached.

```python
def soupServings(N: int) -> float:
    if N >= 4800:  # A large enough N that makes probability close to 1
        return 1.0
    
    memo = {}
    
    def dfs(a, b):
        if (a, b) in memo:
            return memo[(a, b)]
        
        if a <= 0 and b <= 0:
            return 0.5
        if a <= 0:
            return 1.0
        if b <= 0:
            return 0.0
        
        memo[(a, b)] = 0.25 * (dfs(a-100, b) + dfs(a-75, b-25) + dfs(a-50, b-50) + dfs(a-25, b-75))
        return memo[(a, b)]
    
    return dfs(N, N)
```

**Time Complexity:** O(N^2), in the worst case scenario, due to memoization.

**Space Complexity:** O(N^2), due to the space required for storing computed results.

---

### Approach 2: Iterative Dynamic Programming

Instead of using recursion and memoization, we use bottom-up dynamic programming to build up the solution iteratively. This often saves stack space and can be more intuitive in understanding how the solution is constructed sequentially.

#### Intuition:
- Use a 2D array `dp` where `dp[a][b]` represents the probability that soup A is depleted first given `a` and `b`.
- Iteratively compute the value of `dp[a][b]` using precomputed `dp` values for possible operations.

#### Detailed Steps:
1. Initialize the base cases similar to the recursive approach.
2. Use nested loops to fill the `dp` table. Iterate through soup sizes from small to large.
3. Reflect operations (100, 0), (75, 25), (50, 50), and (25, 75) by calculating probabilities using corresponding `dp` values.
4. Optimize space by using a reduced array size if initial `N` is very large, considering only cases where probabilistic changes could occur.

```python
def soupServings(N: int) -> float:
    if N >= 4800:  # A large enough N that makes probability close to 1
        return 1.0
    
    N = (N + 24) // 25  # Scale down the problem to prevent large states
    
    dp = [[0] * (N + 1) for _ in range(N + 1)]
    
    for a in range(N + 1):
        for b in range(N + 1):
            if a == 0:
                dp[a][b] = 1 if b > 0 else 0.5
            elif b == 0:
                dp[a][b] = 0
            else:
                dp[a][b] = 0.25 * (
                    dp[max(0, a-4)][b] +
                    dp[max(0, a-3)][max(0, b-1)] +
                    dp[max(0, a-2)][max(0, b-2)] +
                    dp[max(0, a-1)][max(0, b-3)]
                )
    
    return dp[N][N]
```

**Time Complexity:** O(N^2), iterating through every combination of A and B servings.

**Space Complexity:** O(N^2), due to the space required for storing the dp table.

These approaches showcase two common techniques in dynamic programming, top-down memoization and bottom-up tabulation, both effective in solving this probabilistic simulation problem.

---

