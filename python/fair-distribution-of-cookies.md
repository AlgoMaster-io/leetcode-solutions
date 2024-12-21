# [LeetCode 2305: Fair Distribution of Cookies](https://leetcode.com/problems/fair-distribution-of-cookies/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Backtracking Approach](#backtracking-approach)
3. [Dynamic Programming + Bitmasking Approach](#dynamic-programming-plus-bitmasking-approach)

---

## Brute Force Approach

The problem asks us to distribute cookies across k children such that the maximum load on any child is minimized. The most straightforward approach is to check all possible distributions of cookies to the children and find the one that minimizes the maximum load.

### Intuition

- The brute force approach involves generating all possible divisions of cookies among children. For each division, calculate the load on the child with the maximum cookies. Track the minimum across all divisions.
- This method ensures we account for every possible combination, making it highly inefficient but comprehensive.

### Solution

```python
def distributeCookies(cookies, k):
    def distribute(i, distribution):
        # Base case: if all cookies are considered
        if i == len(cookies):
            return max(distribution)
        
        min_unfairness = float('inf')
        for j in range(k):
            # Distribute cookie i to child j
            distribution[j] += cookies[i]
            
            # Recur to distribute the next cookie
            min_unfairness = min(min_unfairness, distribute(i + 1, distribution))
            
            # Backtrack: remove cookie i from child j
            distribution[j] -= cookies[i]
        
        return min_unfairness

    # Initialize distribution as all zeros
    return distribute(0, [0] * k)
```

### Time Complexity

- \( O(k^N) \): For N cookies and k children, checking all configurations requires k choices for each cookie.
  
### Space Complexity

- \( O(N + k) \): Recursion stack can hold at most N layers, and k storage for distribution list.

---

## Backtracking Approach

This approach refines the brute force method by leveraging certain pruning techniques to skip unnecessary calculations when a configuration cannot lead to an optimal solution.

### Intuition

- Using backtracking, we recursively distribute cookies while keeping track of the current state. We backtrack if the current distribution cannot yield a better solution than already found.
- Pruning: Skip configurations where a child receives more than the current minimum maximum load found.

### Solution

```python
def distributeCookiesBacktracking(cookies, k):
    def backtrack(i, distribution, current_max):
        # Base case
        if i == len(cookies):
            return current_max
        
        min_unfairness = float('inf')
        for j in range(k):
            distribution[j] += cookies[i]
            next_max = max(current_max, distribution[j])
            if next_max < min_unfairness: 
                min_unfairness = min(min_unfairness, backtrack(i + 1, distribution, next_max))
            distribution[j] -= cookies[i]
            
            # Pruning: if a child has no cookies, break to avoid redundant calculation
            if distribution[j] == 0:
                break
        
        return min_unfairness
    
    distribution = [0] * k
    return backtrack(0, distribution, 0)
```

### Time Complexity

- Typically better than \( O(k^N) \) due to pruning but still exponential in nature.
  
### Space Complexity

- \( O(N + k) \): Same as brute force.

---

## Dynamic Programming Plus Bitmasking Approach

This method uses dynamic programming combined with bitmasking to solve the problem more efficiently by caching already solved states.

### Intuition

- The state is captured by the mask of cookies distributed and a dp table to record the minimum maximum over assignments. This helps avoid recalculating for identical subproblems.
- For each assignment, compute the completeness using a bitmask; store the result in a 2D array with masks representing sets of cookies.

### Solution

```python
def distributeCookiesDP(cookies, k):
    n = len(cookies)
    # Create all possible distributions
    dp = [[float('inf')] * (1 << n) for _ in range(k+1)]
    dp[0][0] = 0

    for i in range(1, k+1):
        for mask in range(1 << n):
            curr_sum = 0
            for submask in range(mask, -1, -1):
                submask &= mask  # Ensure submask is a subset of mask
                load = sum(cookies[j] for j in range(n) if (submask & (1 << j)))
                dp[i][mask] = min(dp[i][mask], max(dp[i-1][mask ^ submask], load))
    
    return dp[k][(1 << n) - 1]
```

### Time Complexity

- \( O(k \times 2^N \times N) \): Each state examined across possible cookie distributions.
  
### Space Complexity

- \( O(k \times 2^N) \): DP table for k children and 2^N cookie distributions.

Each approach scales in complexity differently, with trade-offs in time and space influenced by constraints and input sizes typical of competitive programming problems.

