# [Leetcode 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches:
- [Approach 1: Recursion](#approach-1-recursion)
- [Approach 2: Recursion with Memoization](#approach-2-recursion-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

---

### Approach 1: Recursion

**Intuition:**

The problem can be stated as a decision at each step. For a given position in the target string `s3`, we have two choices:
1. Use the character from the next position in `s1`.
2. Use the character from the next position in `s2`.

We will try both choices and proceed recursively for the rest of the strings. If we reach the end of both `s1` and `s2` such that we've formed `s3`, then interleaving is possible.

```javascript
function isInterleave(s1, s2, s3) {
    if (s1.length + s2.length !== s3.length) return false;

    function dfs(i, j, k) {
        // If all characters of s3 are matched, we've found a valid interleaving
        if (k === s3.length) return true;

        // Check if we can take character from s1
        if (i < s1.length && s1[i] === s3[k]) {
            if (dfs(i + 1, j, k + 1)) return true;
        }

        // Check if we can take character from s2
        if (j < s2.length && s2[j] === s3[k]) {
            if (dfs(i, j + 1, k + 1)) return true;
        }

        // If neither choice leads to a valid interleaving
        return false;
    }

    return dfs(0, 0, 0);
}
```

**Complexity Analysis:**

- **Time Complexity:** O(2^(m+n)) in the worst case due to the two recursive calls per step, where `m` and `n` are the lengths of `s1` and `s2` respectively.
- **Space Complexity:** O(m + n) due to the recursion stack.

---

### Approach 2: Recursion with Memoization

**Intuition:**

To optimize the recursive solution, we can use a memoization technique. We'll store the results of already computed states in a dictionary to avoid redundant calculations.

```javascript
function isInterleave(s1, s2, s3) {
    if (s1.length + s2.length !== s3.length) return false;

    const memo = {};
    
    function dfs(i, j, k) {
        if (k === s3.length) return true;

        const key = `${i},${j}`;
        if (key in memo) return memo[key];

        let valid = false;

        if (i < s1.length && s1[i] === s3[k]) {
            valid = dfs(i + 1, j, k + 1);
        }

        if (!valid && j < s2.length && s2[j] === s3[k]) {
            valid = dfs(i, j + 1, k + 1);
        }

        memo[key] = valid;
        return valid;
    }

    return dfs(0, 0, 0);
}
```

**Complexity Analysis:**

- **Time Complexity:** O(m * n) since each unique pair `(i, j)` is calculated only once.
- **Space Complexity:** O(m * n) for storing results of states in the memo object and the recursion stack.

---

### Approach 3: Dynamic Programming

**Intuition:**

We can use a DP table to iteratively solve the problem. The DP table `dp[i][j]` will be `true` if the first `i` characters of `s1` and the first `j` characters of `s2` can form the first `i+j` characters of `s3`.

- Base Case: `dp[0][0]` is `true` because with no characters from `s1` and `s2`, only an empty `s3` can be formed.
- Transition:  
  If the last character matches with `s1`, check `dp[i-1][j]`;  
  If the last character matches with `s2`, check `dp[i][j-1]`.

```javascript
function isInterleave(s1, s2, s3) {
    if (s1.length + s2.length !== s3.length) return false;

    const m = s1.length, n = s2.length;
    const dp = Array(m + 1).fill(null).map(() => Array(n + 1).fill(false));

    dp[0][0] = true;

    for (let i = 0; i <= m; i++) {
        for (let j = 0; j <= n; j++) {
            if (i > 0 && s1[i - 1] === s3[i + j - 1]) {
                dp[i][j] = dp[i][j] || dp[i - 1][j];
            }
            if (j > 0 && s2[j - 1] === s3[i + j - 1]) {
                dp[i][j] = dp[i][j] || dp[i][j - 1];
            }
        }
    }

    return dp[m][n];
}
```

**Complexity Analysis:**

- **Time Complexity:** O(m * n) where `m` and `n` are the lengths of `s1` and `s2` respectively, as we are filling the DP table.
- **Space Complexity:** O(m * n) for the DP table.

---

Each approach gets progressively more optimal by reducing redundant calculations and utilizing efficient data structures to store and compute results.

