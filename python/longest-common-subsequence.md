# [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Memoization (Top-Down DP) Approach](#memoization-approach)
- [Tabulation (Bottom-Up DP) Approach](#tabulation-approach)
- [Space Optimized DP Approach](#space-optimized-dp-approach)

---

### Recursive Approach

The most straightforward approach is the recursive method. We try to build the solution by considering both strings from the end. If the current characters of both strings are the same, they are part of the LCS; otherwise, we take the maximum of ignoring the current character from either of the strings.

#### Intuition:
- If characters at the current indices match, the LCS length is 1 plus the LCS length excluding these characters.
- If they do not match, we calculate LCS in two scenarios: excluding the current character of the first string and excluding the current character of the second string. The result is the maximum of these two values.

**Python Code:**

```python
def longestCommonSubsequence(text1: str, text2: str) -> int:
    def lcs_recursive(i, j):
        # Base case: when either string is exhausted
        if i == len(text1) or j == len(text2):
            return 0
        # If characters match, move both indices
        if text1[i] == text2[j]:
            return 1 + lcs_recursive(i + 1, j + 1)
        # Otherwise, try both possibilities and take the max
        else:
            return max(lcs_recursive(i + 1, j), lcs_recursive(i, j + 1))
    
    return lcs_recursive(0, 0)
```

**Complexity:**
- Time: O(2^n), where n is the length of the longer string. This is due to the exponential number of recursive calls.
- Space: O(n + m) for the recursion stack, where n and m are lengths of the two strings.

---

### Memoization Approach

This approach stores results of subproblems to avoid redundant calculations, transforming the exponential approach to polynomial time.

#### Intuition:
- Use a 2D table (`memo`) where `memo[i][j]` represents the LCS of strings `text1[i:]` and `text2[j:]`.
- If `memo[i][j]` has been computed, return it directly instead of recalculating.

**Python Code:**

```python
def longestCommonSubsequence(text1: str, text2: str) -> int:
    memo = {}
    
    def lcs_memo(i, j):
        # Base case
        if i == len(text1) or j == len(text2):
            return 0
        # Check if solution already known
        if (i, j) in memo:
            return memo[(i, j)]
        
        if text1[i] == text2[j]:
            memo[(i, j)] = 1 + lcs_memo(i + 1, j + 1)
        else:
            memo[(i, j)] = max(lcs_memo(i + 1, j), lcs_memo(i, j + 1))
        
        return memo[(i, j)]
    
    return lcs_memo(0, 0)
```

**Complexity:**
- Time: O(n*m) because each unique pair of indices is computed once.
- Space: O(n*m) to store the memoized results.

---

### Tabulation Approach

This dynamic programming approach fills a table iteratively from the bottom-up.

#### Intuition:
- Similar to memoization, but we solve from the smallest subproblems up.
- Use a DP table (`dp[i][j]`), where each cell represents the length of LCS for substrings `text1[0:i]` and `text2[0:j]`.

**Python Code:**

```python
def longestCommonSubsequence(text1: str, text2: str) -> int:
    # Create the DP table
    dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]

    # Fill the DP table
    for i in range(1, len(text1) + 1):
        for j in range(1, len(text2) + 1):
            # If the current characters match
            if text1[i - 1] == text2[j - 1]:
                dp[i][j] = 1 + dp[i - 1][j - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    # DP solution is in the last cell
    return dp[len(text1)][len(text2)]
```

**Complexity:**
- Time: O(n*m)
- Space: O(n*m)

---

### Space Optimized DP Approach

We can optimize the space from O(n*m) to O(min(n, m)) by only maintaining two arrays.

#### Intuition:
- At any point, we only need values from the current and previous row. 
- Two rolling arrays can replace the entire DP table.

**Python Code:**

```python
def longestCommonSubsequence(text1: str, text2: str) -> int:
    if len(text1) < len(text2):
        text1, text2 = text2, text1
    
    # Initialize the previous row
    prev = [0] * (len(text2) + 1)

    # Simulate the DP table row by row
    for i in range(1, len(text1) + 1):
        current = [0] * (len(text2) + 1)
        for j in range(1, len(text2) + 1):
            if text1[i - 1] == text2[j - 1]:
                current[j] = 1 + prev[j - 1]
            else:
                current[j] = max(prev[j], current[j - 1])
        prev = current

    return prev[len(text2)]
```

**Complexity:**
- Time: O(n*m)
- Space: O(min(n, m)), as we only store two rows, whichever string is shorter.

