# [Leetcode Problem 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

## Recursive Approach

### Intuition
The recursive approach explores all possible ways to interleave `s1` and `s2` to form `s3`. It checks if the current character of `s3` can be formed by the current characters in `s1` and `s2`, and recursively calls itself for the next characters.

### Solution
```python
def isInterleave(s1: str, s2: str, s3: str) -> bool:
    # If the total lengths of s1 and s2 don't match s3, it's impossible.
    if len(s1) + len(s2) != len(s3):
        return False

    # Recursive function to check interleaving
    def check(i: int, j: int, k: int) -> bool:
        # If we have traversed all characters of s3, return True
        if k == len(s3):
            return True

        valid = False

        # Check if the current character of s3 matches the current character of s1
        if i < len(s1) and s1[i] == s3[k]:
            valid = check(i+1, j, k+1)

        # Check if the current character of s3 matches the current character of s2
        if j < len(s2) and s2[j] == s3[k]:
            valid = valid or check(i, j+1, k+1)

        return valid

    # Start recursion
    return check(0, 0, 0)
```

### Complexity
- **Time Complexity**: O(2^(m+n)), where m is the length of `s1` and n is the length of `s2`.
- **Space Complexity**: O(m+n), due to recursion stack space.

## Memoization Approach

### Intuition
To optimize the recursive solution, we can use memoization to cache the results of subproblems to avoid repetitive calculations. This speeds up the process by storing results of overlapping subproblems.

### Solution
```python
def isInterleave(s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3):
        return False

    from functools import lru_cache

    @lru_cache(None)
    def check(i: int, j: int, k: int) -> bool:
        if k == len(s3):
            return True

        valid = False
        if i < len(s1) and s1[i] == s3[k]:
            valid = check(i+1, j, k+1)
        if j < len(s2) and s2[j] == s3[k]:
            valid = valid or check(i, j+1, k+1)

        return valid

    return check(0, 0, 0)
```

### Complexity
- **Time Complexity**: O(m*n), due to storing subproblem solutions for each pair (i, j).
- **Space Complexity**: O(m*n), for the cache storage.

## Dynamic Programming Approach

### Intuition
We can further optimize by employing dynamic programming. We build a 2D table `dp` where `dp[i][j]` will be True if `s3[0:i+j]` can be formed by interleaving `s1[0:i]` and `s2[0:j]`.

### Solution
```python
def isInterleave(s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3):
        return False

    # dp[i][j] will be True if s3[0:i+j] is formed by interleaving s1[0:i] and s2[0:j]
    dp = [[False] * (len(s2) + 1) for _ in range(len(s1) + 1)]
    dp[0][0] = True

    # Fill the first column where s1 contributes to s3
    for i in range(1, len(s1) + 1):
        dp[i][0] = dp[i-1][0] and s1[i-1] == s3[i-1]

    # Fill the first row where s2 contributes to s3
    for j in range(1, len(s2) + 1):
        dp[0][j] = dp[0][j-1] and s2[j-1] == s3[j-1]

    # Fill the rest of the dp table
    for i in range(1, len(s1) + 1):
        for j in range(1, len(s2) + 1):
            # Check if current position matches either option
            dp[i][j] = (dp[i-1][j] and s1[i-1] == s3[i+j-1]) or (dp[i][j-1] and s2[j-1] == s3[i+j-1])

    # Return the result for the whole strings
    return dp[len(s1)][len(s2)]
```

### Complexity
- **Time Complexity**: O(m*n), filling the dp table.
- **Space Complexity**: O(m*n), storage for the dp table.

