# [Leetcode 115: Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Navigation
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Memoization](#approach-2-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

## Approach 1: Recursive Solution

### Intuition
The core idea of this approach is to recursively find all distinct subsequences of string `s` that match string `t`. We try to match characters from `s` with `t` starting from the first character of `t`. If they match, we have two choices:
1. Consider them as a part of the subsequences.
2. Skip the character from `s` and try to find other possibilities.

### Steps
- Base cases:
  - If the length of `t` becomes zero, we've found a distinct subsequence.
  - If the length of `s` becomes zero while `t` is still not matched, return 0.
- Check if the current character of `s` matches the current character of `t`.
- Recur in both scenarios — matching and skipping the character.

### Code
```typescript
function numDistinct(s: string, t: string): number {
    const recurse = (i: number, j: number): number => {
        // Base case: If `t` is empty
        if (j === t.length) return 1;
        // If `s` is empty while `t` is not completely matched
        if (i === s.length) return 0;

        // If characters of `s` and `t` match
        if (s[i] === t[j]) {
            // Try both matching the current characters and skipping the current character of `s`
            return recurse(i + 1, j + 1) + recurse(i + 1, j);
        }
        // If characters don't match, skip the current character of `s`
        else {
            return recurse(i + 1, j);
        }
    }

    return recurse(0, 0);
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(2^n)\), as each character of `s` can either be chosen or skipped in the subsequence.
- **Space Complexity**: \(O(n)\) due to the recursion stack.

## Approach 2: Memoization

### Intuition
To optimize the recursive approach, we can store the results of already computed subproblems. By using a memoization table, we avoid recalculating results for previously processed indices. This helps us to reduce the exponential time complexity.

### Steps
- Create a DP array `dp[i][j]` where `dp[i][j]` will hold the number of distinct subsequences of `s[i:]` which equals `t[j:]`.
- Populate the memoization table during the recursion.

### Code
```typescript
function numDistinct(s: string, t: string): number {
    const memo: number[][] = Array.from({ length: s.length }, () => Array(t.length).fill(-1));

    const recurse = (i: number, j: number): number => {
        if (j === t.length) return 1;
        if (i === s.length) return 0;

        if (memo[i][j] !== -1) return memo[i][j];

        if (s[i] === t[j]) {
            memo[i][j] = recurse(i + 1, j + 1) + recurse(i + 1, j);
        } else {
            memo[i][j] = recurse(i + 1, j);
        }

        return memo[i][j];
    }

    return recurse(0, 0);
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(n \times m)\), where `n` is the length of `s` and `m` is the length of `t`.
- **Space Complexity**: \(O(n \times m)\) for storing memoization results.

## Approach 3: Dynamic Programming

### Intuition
Using dynamic programming, we iteratively build up the solution by solving smaller subproblems. We maintain a 2D DP table where each entry `dp[i][j]` represents the number of distinct subsequences of `s[i:]` which equal `t[j:]`.

### Steps
- Initialize `dp[i][t.length] = 1` for all `i` because an empty `t` is a subsequence of any string `s`.
- Iterate over `s` and `t` in reverse.
- For each combination of indices, evaluate if `s[i]` matches `t[j]`. Update the DP table accordingly.

### Code
```typescript
function numDistinct(s: string, t: string): number {
    const n = s.length;
    const m = t.length;
    const dp: number[][] = Array.from({ length: n + 1 }, () => Array(m + 1).fill(0));

    // Base case: An empty `t` is a subsequence of any string `s`
    for (let i = 0; i <= n; i++) {
        dp[i][m] = 1;
    }

    // Fill the dp table in reverse
    for (let i = n - 1; i >= 0; i--) {
        for (let j = m - 1; j >= 0; j--) {
            if (s[i] === t[j]) {
                dp[i][j] = dp[i + 1][j + 1] + dp[i + 1][j];
            } else {
                dp[i][j] = dp[i + 1][j];
            }
        }
    }

    return dp[0][0];
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(n \times m)\), where `n` is the length of `s` and `m` is the length of `t`.
- **Space Complexity**: \(O(n \times m)\) for the DP table.

