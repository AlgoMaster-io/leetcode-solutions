# [Leetcode 392: Is Subsequence](https://leetcode.com/problems/is-subsequence/)

Approaches:
- [Approach 1: Two-pointer technique](#approach-1-two-pointer-technique)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

## Approach 1: Two-pointer technique

### Intuition
The simplest way to determine if one string (`s`) is a subsequence of another string (`t`) is by using a two-pointer technique:
1. Initialize two pointers, one for each string.
2. Traverse through string `t` with one pointer.
3. Each time the current character in `t` matches the current character in `s`, increment the pointer for `s`.
4. If the pointer for `s` reaches the end of `s`, then `s` is a subsequence of `t`.

### Detailed Steps
- Start with two pointers, one for `s` (let's call it `i`) and another for `t` (let's call it `j`).
- Traverse through `t` using pointer `j`.
- Whenever `s[i]` equals `t[j]`, move the pointer `i` forward.
- If `i` equals the length of `s`, that means every character in `s` was found sequentially in `t`.

### Code
```go
func isSubsequence(s string, t string) bool {
    // i keeps track of where we are in s
    i := 0

    // Go through each character in t
    for j := 0; j < len(t); j++ {
        // If characters match, move the pointer in s
        if i < len(s) && s[i] == t[j] {
            i++
        }
        // If we've matched the entire s, early return true
        if i == len(s) {
            return true
        }
    }

    // If we finish looping without matching all of s, return false
    return i == len(s)
}
```

### Time Complexity
- **O(n + m)**: We process each character in `t` at most once and the same for `s`. Here `n` is the length of `t` and `m` is the length of `s`.

### Space Complexity
- **O(1)**: We only use a constant amount of extra space.

## Approach 2: Dynamic Programming

### Intuition
Dynamic programming is typically used when you want to solve overlapping subproblems or optimize recursive relations. Here, it's overkill for the problem constraints but could be considered if `s` and `t` were extremely large or if `t` was reused for multiple `s` queries.

### Detailed Steps
- Construct a DP table where `dp[i][j]` indicates whether `s[0..i-1]` is a subsequence of `t[0..j-1]`.
- If `s[i-1] == t[j-1]`, then `dp[i][j] = dp[i-1][j-1]`.
- Otherwise, `dp[i][j] = dp[i][j-1]`.
- Initialize `dp[0][j]` as `true` since an empty string is a subsequence of any string.

### Code
```go
func isSubsequence(s string, t string) bool {
    m, n := len(s), len(t)
    
    // Create a DP table of size (m+1) by (n+1)
    dp := make([][]bool, m+1)
    for i := 0; i <= m; i++ {
        dp[i] = make([]bool, n+1)
    }

    // Empty string is a subsequence of any prefix of t
    for j := 0; j <= n; j++ {
        dp[0][j] = true
    }

    // Fill in the table
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if s[i-1] == t[j-1] {
                dp[i][j] = dp[i-1][j-1]
            } else {
                dp[i][j] = dp[i][j-1]
            }
        }
    }

    // Result is whether the entire s can be a subsequence of t
    return dp[m][n]
}
```

### Time Complexity
- **O(m * n)**: We fill a table with dimensions `m` by `n`.

### Space Complexity
- **O(m * n)**: We use a 2D DP table to store results for substring comparisons.

