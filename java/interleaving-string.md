[LeetCode Problem 97: Interleaving String](https://leetcode.com/problems/interleaving-string/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Memoization (Top-Down Dynamic Programming) Approach](#memoization-approach)
3. [Iterative Dynamic Programming Approach](#iterative-dp-approach)

### Recursive Approach

#### Intuition
The recursive approach attempts to build the interleaved string one character at a time. At each step, we decide whether to take the next character from `s1` or `s2`, and check recursively if taking that character leads to a solution. If both choices are possible, we try both.

#### Steps
1. Define a recursive function that takes three indices, one for each of `s1`, `s2`, and `s3`.
2. If both indices for `s1` and `s2` are at the start of their respective strings and `s3` is also at the beginning, then `s1` and `s2` are interleaving `s3`.
3. If the current character of `s1` matches with `s3`, make a recursive call by incrementing the index of `s1` and `s3`.
4. If the current character of `s2` matches with `s3`, make a recursive call by incrementing the index of `s2` and `s3`.
5. If either of the recursive calls return true, then `s1` and `s2` can interleave `s3`.

#### Code

```java
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        return isInterleaveRecursive(s1, 0, s2, 0, s3, 0);
    }

    private boolean isInterleaveRecursive(String s1, int i, String s2, int j, String s3, int k) {
        if (i == s1.length() && j == s2.length() && k == s3.length()) {
            return true;
        }
        if (k == s3.length()) {
            return false;
        }
        boolean interleave = false;
        if (i < s1.length() && s1.charAt(i) == s3.charAt(k)) {
            interleave = isInterleaveRecursive(s1, i + 1, s2, j, s3, k + 1);
        }
        if (!interleave && j < s2.length() && s2.charAt(j) == s3.charAt(k)) {
            interleave = isInterleaveRecursive(s1, i, s2, j + 1, s3, k + 1);
        }
        return interleave;
    }
}
```

#### Complexity
- **Time Complexity:** O(2^(m+n)), where m and n are the lengths of `s1` and `s2` respectively. The recursive tree depth is `m + n`.
- **Space Complexity:** O(m+n), which is the maximal depth of recursive calls.

### Memoization (Top-Down DP) Approach

#### Intuition
The recursive solution recalculates solutions for overlapping subproblems, leading to inefficiencies. By storing previously calculated results in a 2D array, we can optimize the recursive solution using memoization.

#### Steps
1. Use a 2D array `memo` where `memo[i][j]` indicates whether `s1[i:]` and `s2[j:]` can interleave into `s3[i+j:]`.
2. Follow the same logic as the recursive approach, but store results in `memo` before making recursive calls, and check `memo` before calculating new results.

#### Code

```java
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        int[][] memo = new int[s1.length() + 1][s2.length() + 1];
        return isInterleave(s1, 0, s2, 0, s3, 0, memo);
    }

    private boolean isInterleave(String s1, int i, String s2, int j, String s3, int k, int[][] memo) {
        if (i == s1.length() && j == s2.length() && k == s3.length()) {
            return true;
        }
        if (memo[i][j] != 0) {
            return memo[i][j] == 1;
        }
        boolean interleave = false;
        if (i < s1.length() && s1.charAt(i) == s3.charAt(k)) {
            interleave = isInterleave(s1, i + 1, s2, j, s3, k + 1, memo);
        }
        if (!interleave && j < s2.length() && s2.charAt(j) == s3.charAt(k)) {
            interleave = isInterleave(s1, i, s2, j + 1, s3, k + 1, memo);
        }
        memo[i][j] = interleave ? 1 : 2;
        return interleave;
    }
}
```

#### Complexity
- **Time Complexity:** O(m * n), since each state `(i, j)` is computed only once.
- **Space Complexity:** O(m * n) for memoization storage.

### Iterative Dynamic Programming Approach

#### Intuition
Transforming the recursive approach into an iterative form using a 2D boolean array can avoid the function call overhead.

#### Steps
1. Create a 2D boolean array `dp` where `dp[i][j]` is true if `s1[0:i]` and `s2[0:j]` can interleave into `s3[0:i+j]`.
2. Initialize `dp[0][0]` to true because empty strings interleave to make an empty string.
3. Iterate over `s1` and `s2` and fill `dp` based on previous results.
4. Return `dp[m][n]`.

#### Code

```java
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) {
            return false;
        }
        boolean[][] dp = new boolean[s1.length() + 1][s2.length() + 1];
        dp[0][0] = true;
        for (int i = 0; i <= s1.length(); i++) {
            for (int j = 0; j <= s2.length(); j++) {
                if (i > 0 && s1.charAt(i - 1) == s3.charAt(i + j - 1)) {
                    dp[i][j] = dp[i - 1][j];
                }
                if (j > 0 && s2.charAt(j - 1) == s3.charAt(i + j - 1)) {
                    dp[i][j] |= dp[i][j - 1];
                }
            }
        }
        return dp[s1.length()][s2.length()];
    }
}
```

#### Complexity
- **Time Complexity:** O(m * n), where m is the length of `s1` and n is the length of `s2`.
- **Space Complexity:** O(m * n) for the DP table. However, this can also be optimized to O(n) by using a rolling array technique.

This iterative approach is more efficient in terms of execution speed and is the go-to method for solving such problems with dynamic programming.

