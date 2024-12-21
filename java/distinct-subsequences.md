# [Distinct Subsequences - Leetcode 115](https://leetcode.com/problems/distinct-subsequences/)

## Approaches
1. [Recursive Solution](#approach-1-recursive-solution)
2. [Memoization (Top-Down Dynamic Programming)](#approach-2-memoization)
3. [Tabulation (Bottom-Up Dynamic Programming)](#approach-3-tabulation)
4. [Optimized Tabulation with Space Reduction](#approach-4-optimized-tabulation)

---

## Approach 1: Recursive Solution

### Intuition:
The basic idea is to recursively try to match each character of the string `t` with the characters in `s`. At each step, we have two choices: either take the character from `s` to match with the current character of `t` or skip the character from `s`. The base cases are defined when `t` becomes empty (which means we found a subsequence) or when `s` becomes empty without fully matching `t`.

### Code:
```java
public class Solution {
    public int numDistinct(String s, String t) {
        return numDistinct(s, t, 0, 0);
    }

    private int numDistinct(String s, String t, int i, int j) {
        // If we've found a subsequence
        if (j == t.length()) return 1;
        // If s is exhausted and t is still not matched
        if (i == s.length()) return 0;

        if (s.charAt(i) == t.charAt(j)) {
            // Include the current character and move both pointers
            // Or move only in `s` to find another match
            return numDistinct(s, t, i + 1, j + 1) + numDistinct(s, t, i + 1, j);
        } else {
            // Skip the current character of `s`
            return numDistinct(s, t, i + 1, j);
        }
    }
}
```

### Complexity:
- **Time Complexity**: O(2^m), where `m` is the length of `s`. The recursive tree can potentially explore all subsets of `s`.
- **Space Complexity**: O(n), where `n` is the depth of the recursion stack, effectively the length of `s`.

---

## Approach 2: Memoization

### Intuition:
The recursive solution contains overlapping subproblems. By storing the results of already computed states (i, j), we can avoid redundant calculations. This transforms the recursion into a top-down dynamic programming approach using a 2D array for memoization.

### Code:
```java
public class Solution {
    public int numDistinct(String s, String t) {
        int[][] memo = new int[s.length()][t.length()];
        for (int[] row : memo) {
            Arrays.fill(row, -1);
        }
        return numDistinct(s, t, 0, 0, memo);
    }

    private int numDistinct(String s, String t, int i, int j, int[][] memo) {
        if (j == t.length()) return 1;
        if (i == s.length()) return 0;
        if (memo[i][j] != -1) return memo[i][j];

        if (s.charAt(i) == t.charAt(j)) {
            memo[i][j] = numDistinct(s, t, i + 1, j + 1, memo) + numDistinct(s, t, i + 1, j, memo);
        } else {
            memo[i][j] = numDistinct(s, t, i + 1, j, memo);
        }
        return memo[i][j];
    }
}
```

### Complexity:
- **Time Complexity**: O(m * n), where `m` is the length of `s` and `n` is the length of `t` because each subproblem `(i, j)` is computed once.
- **Space Complexity**: O(m * n) for the memoization table plus O(n) for the recursion stack.

---

## Approach 3: Tabulation

### Intuition:
Using a bottom-up approach, we can fill up a 2D table where `dp[i][j]` indicates the number of distinct subsequences of `t[0...j-1]` in `s[0...i-1]`. We initialize the first row and first column based on the base conditions and progressively fill the DP table.

### Code:
```java
public class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i <= m; i++) {
            dp[i][0] = 1; // There's one way to match an empty `t`
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    // Add ways with and without current character
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    // Skip the current character
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[m][n];
    }
}
```

### Complexity:
- **Time Complexity**: O(m * n), where `m` is the length of `s` and `n` is the length of `t`.
- **Space Complexity**: O(m * n) for the table.

---

## Approach 4: Optimized Tabulation

### Intuition:
Since at every step we only use the current and previous rows, we can optimize space by maintaining only two rows at a time. Swapping rows reduces space complexity from `O(m*n)` to `O(n)`.

### Code:
```java
public class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length();
        int n = t.length();
        int[] prev = new int[n + 1];
        int[] curr = new int[n + 1];

        // Pre-fill for empty string `t`
        prev[0] = 1;

        for (int i = 1; i <= m; i++) {
            curr[0] = 1; // Reset for new `i` position
            for (int j = 1; j <= n; j++) {
                if (s.charAt(i - 1) == t.charAt(j - 1)) {
                    curr[j] = prev[j - 1] + prev[j];
                } else {
                    curr[j] = prev[j];
                }
            }
            // Swap current and previous for next iteration
            int[] temp = prev;
            prev = curr;
            curr = temp;
        }
        return prev[n];
    }
}
```

### Complexity:
- **Time Complexity**: O(m * n), where `m` is the length of `s` and `n` is the length of `t`.
- **Space Complexity**: O(n) for storing two rows.

---

These are the solutions from basic recursive implementation to optimized dynamic programming approaches for the Distinct Subsequences problem. The optimal approaches make effective use of dynamic programming to reduce both time and space complexity.

