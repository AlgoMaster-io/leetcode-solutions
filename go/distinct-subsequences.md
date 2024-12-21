# [Leetcode 115: Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)

## Solutions
- [Brute Force Recursion](#brute-force-recursion)
- [Dynamic Programming (Memoization)](#dynamic-programming-memoization)
- [Dynamic Programming (Tabulation)](#dynamic-programming-tabulation)

---

### Brute Force Recursion
The brute force approach to solve this problem is to use recursion. Given two strings `s` and `t`, the main idea is that for each character in `s`, we have two choices: either to include it in the current sequence if it matches the current character in `t` or to skip it. 

The goal is to find all possible ways in which `t` appears as a subsequence in `s`.

#### Intuition
- Start with indices at the beginning of both strings.
- If a character in `s` matches a character in `t`, explore two paths:
  - Include this matching character and move the index in `t`.
  - Skip the current character and move to the next character in `s`.
- If you reach the end of `t`, it means one valid subsequence is found.
- If you reach the end of `s` before `t`, return 0 as `t` cannot be completely formed.

```go
func numDistinctRec(s string, t string) int {
    var helper func(si, ti int) int
    helper = func(si, ti int) int {
        // If all characters of t are matched
        if ti == len(t) {
            return 1
        }
        // If s is exhausted without matching all characters of t
        if si == len(s) {
            return 0
        }
        // Option to skip the current character of s
        result := helper(si+1, ti)
        // Option to match current character of s with t
        if s[si] == t[ti] {
            result += helper(si+1, ti+1)
        }
        return result
    }
    return helper(0, 0)
}
```

**Time Complexity**: O(2^n) - Every character in `s` gives us two choices leading to an exponential number of combinations.

**Space Complexity**: O(n) - For the recursion stack.

---

### Dynamic Programming (Memoization)
The recursive approach calculates the same subproblems multiple times. By storing the results of these subproblems, we can reduce the computation time significantly. This is known as memoization.

#### Intuition
- Use a 2D slice `dp` where `dp[i][j]` represents the number of distinct subsequences of `t[j:]` in `s[i:]`.
- Use recursion with the added benefit of storing intermediate results in `dp`.

```go
func numDistinctMemo(s string, t string) int {
    dp := make([][]int, len(s)+1)
    for i := range dp {
        dp[i] = make([]int, len(t)+1)
        for j := range dp[i] {
            dp[i][j] = -1 // Uncomputed state
        }
    }

    var helper func(si, ti int) int
    helper = func(si, ti int) int {
        if ti == len(t) {
            return 1
        }
        if si == len(s) {
            return 0
        }
        if dp[si][ti] != -1 {
            return dp[si][ti]
        }
        result := helper(si+1, ti)
        if s[si] == t[ti] {
            result += helper(si+1, ti+1)
        }
        dp[si][ti] = result
        return result
    }
    return helper(0, 0)
}
```

**Time Complexity**: O(n * m) - Where `n` is the length of `s` and `m` is the length of `t`, with each subproblem solved once.

**Space Complexity**: O(n * m) - For the memoization array.

---

### Dynamic Programming (Tabulation)
Instead of recursion, we can use a bottom-up approach to fill a table. 

#### Intuition
- Create a (len(s) + 1) x (len(t) + 1) table `dp`.
- `dp[i][j]` signifies the number of ways to form `t[j:]` from `s[i:]`.
- Initialize `dp` such that every `dp[i][len(t)] = 1` because an empty `t` can be formed in one way (by selecting nothing).
- Compute values from the last index backward to the first.

```go
func numDistinctTab(s string, t string) int {
    dp := make([][]int, len(s)+1)
    for i := range dp {
        dp[i] = make([]int, len(t)+1)
    }
    // Initialization: dp[i][len(t)] = 1, because "" is a subsequence of any s
    for i := 0; i <= len(s); i++ {
        dp[i][len(t)] = 1
    }
    // Fill the dp table from bottom to top
    for i := len(s) - 1; i >= 0; i-- {
        for j := len(t) - 1; j >= 0; j-- {
            dp[i][j] = dp[i+1][j]
            if s[i] == t[j] {
                dp[i][j] += dp[i+1][j+1]
            }
        }
    }
    return dp[0][0]
}
```

**Time Complexity**: O(n * m) - Using a grid to compute values in a linear manner for each entry.

**Space Complexity**: O(n * m) - The storage needed for the DP table.

---

By employing dynamic programming, we transform an exponential time complexity problem into a polynomial one, making it feasible for large inputs. Each of the outlined methods progressively improves upon the previous in terms of efficiency and clarity.

