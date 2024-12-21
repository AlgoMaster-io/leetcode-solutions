# [Leetcode 44: Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)

## Approaches:
1. [Recursive Approach](#recursive-approach)
2. [Memoization Approach](#memoization-approach)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

---

## Recursive Approach

The recursive approach uses backtracking to match the string `s` against the pattern `p`. This approach considers every possible scenario where characters match, or where wildcard characters ('*' or '?') are used, effectively trying to match the remainder of both strings.

### Intuition:
We recursively check:
- Characters match directly or with '?'
- Skipping characters with '*' allowing it to match zero or more characters

### TypeScript Code:
```typescript
function isMatchRecursive(s: string, p: string): boolean {
    function match(i: number, j: number): boolean {
        if (j === p.length) return i === s.length;
        if (i < s.length && (p[j] === s[i] || p[j] === '?')) {
            // '?' matches any single character
            return match(i + 1, j + 1);
        }
        if (p[j] === '*') {
            // '*' matches any sequence of characters (including empty)
            return match(i, j + 1) || (i < s.length && match(i + 1, j));
        }
        return false;
    }
    
    return match(0, 0);
}
```

### Complexity:
- **Time Complexity**: `O(2^(m+n))` where `m` and `n` are the lengths of strings `s` and `p` respectively, due to exponential recursion.
- **Space Complexity**: `O(m+n)`, the maximum depth of recursion stack.

---

## Memoization Approach

To optimize the recursive approach, memoization is used where results of overlapping sub-problems are stored. This avoids recalculating results for the same `i` and `j`.

### Intuition:
By storing results of subproblems in a 2D array, we can significantly reduce redundant calculations.

### TypeScript Code:
```typescript
function isMatchMemo(s: string, p: string): boolean {
    const memo: boolean[][] = Array(s.length + 1).fill(null).map(() => Array(p.length + 1).fill(undefined));

    function match(i: number, j: number): boolean {
        if (memo[i][j] !== undefined) return memo[i][j];
        if (j === p.length) return i === s.length;
        if (i < s.length && (p[j] === s[i] || p[j] === '?')) {
            memo[i][j] = match(i + 1, j + 1);
            return memo[i][j];
        }
        if (p[j] === '*') {
            memo[i][j] = match(i, j + 1) || (i < s.length && match(i + 1, j));
            return memo[i][j];
        }
        memo[i][j] = false;
        return false;
    }
    
    return match(0, 0);
}
```

### Complexity:
- **Time Complexity**: `O(m * n)`, since we compute each state once.
- **Space Complexity**: `O(m * n)` for the memoization array.

---

## Dynamic Programming Approach

The Dynamic Programming approach creates a dp table where `dp[i][j]` represents if the first `i` characters in `s` can match with the first `j` characters in `p`.

### Intuition:
By iteratively filling out a table, we can leverage previously calculated results to build up to the full problem solution more efficiently than recursive approaches.

### TypeScript Code:
```typescript
function isMatchDP(s: string, p: string): boolean {
    const dp: boolean[][] = Array(s.length + 1).fill(null).map(() => Array(p.length + 1).fill(false));
    dp[0][0] = true;

    // Initialize dp table for patterns that start with '*'
    for (let j = 1; j <= p.length; j++) {
        if (p[j - 1] === '*') {
            dp[0][j] = dp[0][j - 1];
        }
    }

    for (let i = 1; i <= s.length; i++) {
        for (let j = 1; j <= p.length; j++) {
            if (p[j - 1] === '*') {
                dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
            } else if (p[j - 1] === '?' || p[j - 1] === s[i - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            }
        }
    }

    return dp[s.length][p.length];
}
```

### Complexity:
- **Time Complexity**: `O(m * n)`, constructing the dp table.
- **Space Complexity**: `O(m * n)` for the dp table, however, can be optimized to `O(n)` by using a rolling array approach.

In this problem, understanding each approach's trade-offs is crucial. The recursive solution is simple but inefficient, whereas memoization significantly speeds it up. The DP approach offers the most efficient solution using tabulation.

