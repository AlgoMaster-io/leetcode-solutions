# [Leetcode 44: Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

## Solutions
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Greedy Algorithm](#approach-3-greedy-algorithm)

### Approach 1: Recursive Backtracking

#### Intuition
The basic approach for solving this problem is using recursion. We check all possibilities by taking each character from the string `s` and try to match it with the pattern `p`. When we encounter a `*`, we try to match zero or more characters from `s`. For `?`, we match exactly one character. This recursive backtracking approach considers all paths which can be inefficient.

#### Implementation

```python
def is_match(s: str, p: str) -> bool:
    def backtrack(s_idx, p_idx):
        # Base Case: If both string ends, it's a successful match
        if s_idx == len(s) and p_idx == len(p):
            return True
        
        # If pattern ends but string didn't or vice versa
        if p_idx == len(p):
            return s_idx == len(s)
        
        # If current characters match or pattern has '?', 
        # move to the next character in both string and pattern
        if s_idx < len(s) and (p[p_idx] == s[s_idx] or p[p_idx] == '?'):
            return backtrack(s_idx + 1, p_idx + 1)
        
        # If we encounter a '*', we have two choices
        if p[p_idx] == '*':
            # Match zero characters from s or move to next character in s
            return (backtrack(s_idx, p_idx + 1) or
                    (s_idx < len(s) and backtrack(s_idx + 1, p_idx)))
        
        # If no match possible
        return False
    
    return backtrack(0, 0)
```

#### Complexity Analysis

- **Time Complexity:** `O(2^N)`, where `N` is the length of the string `s`.
- **Space Complexity:** `O(N)`, the call stack space used by recursive calls.

### Approach 2: Dynamic Programming

#### Intuition
The recursive approach has a lot of redundant calculations when revisiting the same states. We can optimize it using Dynamic Programming by storing intermediate results for subproblems in a 2D table `dp` where `dp[i][j]` indicates if `s[0:i]` matches `p[0:j]`. We iterate over each character and fill the DP table based on pattern matching rules.

#### Implementation

```python
def is_match(s: str, p: str) -> bool:
    m, n = len(s), len(p)
    dp = [[False] * (n + 1) for _ in range(m + 1)]
    dp[0][0] = True
    
    # Fill the DP for patterns starting with `*`
    for j in range(1, n + 1):
        if p[j - 1] == '*':
            dp[0][j] = dp[0][j - 1]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if p[j - 1] == s[i - 1] or p[j - 1] == '?':
                dp[i][j] = dp[i - 1][j - 1]
            elif p[j - 1] == '*':
                dp[i][j] = dp[i][j - 1] or dp[i - 1][j]
    
    return dp[m][n]
```

#### Complexity Analysis

- **Time Complexity:** `O(N * M)`, where `N` is the length of the string `s` and `M` is the length of pattern `p`.
- **Space Complexity:** `O(N * M)` to store the DP table.

### Approach 3: Greedy Algorithm

#### Intuition
A more optimal approach involves using a greedy algorithm. This is based on the fact that a '*' can match any sequence of characters (including an empty sequence). We can iterate through the string `s` and keep track of the last position a `*` was seen in the pattern `p`. When encountering a mismatch, we retreat to the last known `*`, advancing the position in `s` by one, effectively using the `*` to match more characters.

#### Implementation

```python
def is_match(s: str, p: str) -> bool:
    s_len, p_len = len(s), len(p)
    s_idx, p_idx = 0, 0
    star_idx = -1
    s_tmp_idx = -1
    
    while s_idx < s_len:
        # Match character from both sides or match `?`
        if p_idx < p_len and (p[p_idx] == s[s_idx] or p[p_idx] == '?'):
            s_idx += 1
            p_idx += 1
        # When we encounter `*`, record `* position` and subsequent string index
        elif p_idx < p_len and p[p_idx] == '*':
            star_idx = p_idx
            s_tmp_idx = s_idx
            p_idx += 1
        # Mismatch after `*`, backtrack to last '*' and match one more character with it
        elif star_idx == -1:
            return False
        else:
            p_idx = star_idx + 1
            s_tmp_idx += 1
            s_idx = s_tmp_idx
    
    # Check for remaining characters in pattern
    while p_idx < p_len and p[p_idx] == '*':
        p_idx += 1
    
    return p_idx == p_len
```

#### Complexity Analysis

- **Time Complexity:** `O(N + M)`, where `N` is the length of the string `s` and `M` is the length of pattern `p`.
- **Space Complexity:** `O(1)`, as we are using a constant amount of space.

Each of these approaches has its own merits depending on the context, and understanding the specific pattern and structure of input can guide choosing one over the others.

