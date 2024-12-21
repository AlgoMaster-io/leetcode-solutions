# [Leetcode 44: Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

## Approaches:
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Memoization (Top-Down DP)](#approach-2-memoization-top-down-dp)
- [Approach 3: Dynamic Programming (Bottom-Up DP)](#approach-3-dynamic-programming-bottom-up-dp)
- [Approach 4: Two-Pointer Approach](#approach-4-two-pointer-approach)

### Approach 1: Recursive Backtracking

**Intuition**:  
The simplest approach is to use recursion to explore all possible matching scenarios of the string `s` with the pattern `p`. At each stage, you decide how to match the current characters.

- If characters of both strings match or if there's a '?' in pattern, move both pointers.
- If there's a '*', try to match this '*' with all possible sequences including an empty sequence.
- If characters do not match, this path is invalid.

```javascript
function isMatch(s, p) {
    function backtrack(i, j) {
        // If we have reached the end of the pattern,
        // check if the string is also fully matched
        if (j === p.length) return i === s.length;

        // If the current pattern character is '*',
        // we can either skip this character or match it with current string character
        if (p[j] === '*') {
            // Try to match '*' with an empty sequence OR current character
            return backtrack(i, j + 1) || (i < s.length && backtrack(i + 1, j));
        }

        // Current characters are either matching or it is '?'
        if (i < s.length && (p[j] === s[i] || p[j] === '?')) {
            return backtrack(i + 1, j + 1);
        }

        // Characters do not match
        return false;
    }

    return backtrack(0, 0);
}
```

- **Time Complexity**: O(2^(n+m)), where `n` is the length of `s` and `m` is the length of `p`. This is because each step may branch into two recursive calls.
- **Space Complexity**: O(n+m), the recursive stack space.

### Approach 2: Memoization (Top-Down DP)

**Intuition**:  
We can optimize the recursive solution by storing results of already computed subproblems to avoid redundant calculations.

```javascript
function isMatch(s, p) {
    const dp = {};

    function backtrack(i, j) {
        const key = `${i},${j}`;
        if (key in dp) return dp[key];

        if (j === p.length) return i === s.length;

        if (p[j] === '*') {
            dp[key] = backtrack(i, j + 1) || (i < s.length && backtrack(i + 1, j));
        } else {
            dp[key] = (i < s.length && (p[j] === s[i] || p[j] === '?')) && backtrack(i + 1, j + 1);
        }

        return dp[key];
    }

    return backtrack(0, 0);
}
```

- **Time Complexity**: O(n*m), where `n` is the length of `s` and `m` is the length of `p`.
- **Space Complexity**: O(n*m), the space used to store the results of intermediate subproblems in the memo table.

### Approach 3: Dynamic Programming (Bottom-Up DP)

**Intuition**:  
Use a 2D DP table where `dp[i][j]` indicates if the first `i` characters in `s` can match the first `j` characters in `p`.

- If `p[j-1]` is `'*'`, `dp[i][j]` can be true if either `dp[i-1][j]` or `dp[i][j-1]` is true.
- If `p[j-1]` is `?` or matches `s[i-1]`, then `dp[i][j] = dp[i-1][j-1]`
- Initialize `dp[0][0]` to true as empty pattern matches empty string.

```javascript
function isMatch(s, p) {
    const n = s.length;
    const m = p.length;
    const dp = Array.from({ length: n + 1 }, () => Array(m + 1).fill(false));

    dp[0][0] = true;

    // Initial filling for patterns with *
    for (let j = 1; j <= m; j++) {
        if (p[j - 1] === '*') {
            dp[0][j] = dp[0][j - 1];
        }
    }

    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= m; j++) {
            if (p[j - 1] === '?') {
                dp[i][j] = dp[i - 1][j - 1];
            } else if (p[j - 1] === '*') {
                dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
            } else {
                dp[i][j] = s[i - 1] === p[j - 1] && dp[i - 1][j - 1];
            }
        }
    }

    return dp[n][m];
}
```

- **Time Complexity**: O(n*m)
- **Space Complexity**: O(n*m), space taken by the DP table.

### Approach 4: Two-Pointer Approach

**Intuition**:  
Optimize further by using two pointers with tracking for last '*' position. This is more intuitive and reduces space usage.

```javascript
function isMatch(s, p) {
    let sIdx = 0, pIdx = 0;
    let starIdx = -1, sTmpIdx = -1;
  
    while (sIdx < s.length) {
        if (pIdx < p.length && (p[pIdx] === '?' || s[sIdx] === p[pIdx])) {
            sIdx++;
            pIdx++;
        } else if (pIdx < p.length && p[pIdx] === '*') {
            starIdx = pIdx;
            sTmpIdx = sIdx;
            pIdx++;
        } else if (starIdx !== -1) {
            pIdx = starIdx + 1;
            sTmpIdx++;
            sIdx = sTmpIdx;
        } else {
            return false;
        }
    }
  
    while (pIdx < p.length && p[pIdx] === '*') {
        pIdx++;
    }
  
    return pIdx === p.length;
}
```

- **Time Complexity**: O(n+m)
- **Space Complexity**: O(1)

