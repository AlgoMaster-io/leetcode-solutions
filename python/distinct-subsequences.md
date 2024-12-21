# [115. Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Memoization Approach](#memoization-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

### Recursive Approach

To solve the problem of finding distinct subsequences of string `s` that equal string `t`, we can use a recursive approach, which explores all possible subsequences. Here's the intuition and explanation of the approach:

1. **Base Case**: 
   - If the length of `t` is 0, then by definition, the only subsequence is the empty sequence, so we return 1.
   - If the length of `s` is 0 but `t` is not empty, it's impossible to form `t`, so we return 0.

2. **Recursive Case**:
   - If the last character of `s` matches with the last character of `t`, we have two choices:
     - Include this character in the subsequence.
     - Do not include this character in the subsequence.
   - If the last character does not match, we can only proceed by excluding this character of `s`.

3. **Recursion**:
   - Continue solving the subproblems recursively. 

```python
def numDistinct(s, t):
    def recurse(i, j):
        # Base cases
        if j == 0:  # If t is empty, there's exactly one subsequence: the empty one
            return 1
        if i == 0:  # If s is empty and t is not, no possible subsequences
            return 0
        
        if s[i - 1] == t[j - 1]:
            # Either take this match or leave it
            return recurse(i - 1, j - 1) + recurse(i - 1, j)
        else:
            # Must leave it
            return recurse(i - 1, j)

    return recurse(len(s), len(t))
```

**Time Complexity**: O(2^n), where n is the length of string `s`.

**Space Complexity**: O(n), the depth of the recursive stack.

### Memoization Approach

To optimize the recursive solution, we use memoization to avoid recalculating results for the same function calls. 

**Intuition**:

- Use a 2D array `memo` where `memo[i][j]` holds the number of distinct subsequences for `s[:i]` and `t[:j]`.

- Solve the problem recursively while storing already computed results.

```python
def numDistinct(s, t):
    memo = {}

    def recurse(i, j):
        # Check memo to avoid redundant calculations
        if (i, j) in memo:
            return memo[(i, j)]

        if j == 0:
            return 1
        if i == 0:
            return 0

        if s[i - 1] == t[j - 1]:
            memo[(i, j)] = recurse(i - 1, j - 1) + recurse(i - 1, j)
        else:
            memo[(i, j)] = recurse(i - 1, j)

        return memo[(i, j)]

    return recurse(len(s), len(t))
```

**Time Complexity**: O(m*n), where m and n are the lengths of `s` and `t`, respectively.

**Space Complexity**: O(m*n), due to memoization storage.

### Dynamic Programming Approach

The above memoized recursion can be transformed into an iterative dynamic programming solution with a 2D table.

**Intuition**:

- Use a 2D DP table `dp` where `dp[i][j]` represents the number of distinct subsequences of `s[:i]` that form `t[:j]`.

- Initialize `dp[i][0] = 1` for all `i`, since an empty `t` can be formed by any prefix of `s`.

- Fill the table row by row based on character matches or mismatches.

```python
def numDistinct(s, t):
    m, n = len(s), len(t)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Base case initialization
    for i in range(m + 1):
        dp[i][0] = 1

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s[i - 1] == t[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j]
            else:
                dp[i][j] = dp[i - 1][j]
    
    return dp[m][n]
```

**Time Complexity**: O(m*n), where m and n are the lengths of `s` and `t`.

**Space Complexity**: O(m*n), for the DP table.

