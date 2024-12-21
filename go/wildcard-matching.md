# [Wildcard Matching - Leetcode 44](https://leetcode.com/problems/wildcard-matching/)

## Approaches
- [Recursive Approach](#recursive-approach)
- [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Recursive Approach

#### Intuition
The problem requires matching a pattern `p` with a string `s` using two wildcards: `?` which matches any single character, and `*` which matches any sequence of characters (including an empty sequence). A basic approach is to use recursion, considering each character in the string and the pattern:

- If both characters in `s` and `p` are the same or if the pattern has `?`, we move to the next character in both `s` and `p`.
- If the pattern has `*`, it can match:
  - Zero characters: Move to the next character in `p` but stay on the same character in `s`.
  - One or more characters: Move to the next character in `s` but stay on the same character in `p`.

#### Code
```go
func isMatch(s string, p string) bool {
    return matchHelper(s, p, 0, 0)
}

func matchHelper(s, p string, si, pi int) bool {
    // If we reach the end of both strings, it's a match
    if si == len(s) && pi == len(p) {
        return true
    }
    // If we reach the end of the pattern but not string, it's not a match
    if pi == len(p) {
        return false
    }
    // If the pattern character is '*' it can represent an empty sequence or any sequence
    if p[pi] == '*' {
        // Try to match '*' with empty sequence or consume one character of the string
        return matchHelper(s, p, si, pi+1) || (si < len(s) && matchHelper(s, p, si+1, pi))
    }
    // If the pattern character is '?' or it matches the string character, move both pointers
    if si < len(s) && (p[pi] == '?' || p[pi] == s[si]) {
        return matchHelper(s, p, si+1, pi+1)
    }
    // Characters do not match
    return false
}
```

#### Complexity Analysis
- **Time Complexity**: O(2^(m+n)), where `m` is the length of the string, and `n` is the length of the pattern. This is because each '*' can either match no character or one/more characters, leading to an exponential growth in recursive calls.
- **Space Complexity**: O(m+n) due to the stack space used by the recursion.

---

### Dynamic Programming Approach

#### Intuition
To optimize the recursive approach and handle overlapping subproblems, we apply dynamic programming. We use a 2D DP table `dp[i][j]` where `dp[i][j]` denotes whether the first `i` characters in `s` match the first `j` characters in `p`.

- **Base case**: `dp[0][0]` is `true` because two empty strings match.
- If the pattern character is `*`, we can ignore it or use it to match the current string character.
- If the pattern character is `?` or it matches the string character, inherit the result of matching previous characters.

#### Code
```go
func isMatch(s string, p string) bool {
    m, n := len(s), len(p)
    dp := make([][]bool, m+1)
    for i := range dp {
        dp[i] = make([]bool, n+1)
    }
    dp[0][0] = true // both pattern and string are empty
    
    for j := 1; j <= n; j++ { // only '*' can match an empty string
        if p[j-1] == '*' {
            dp[0][j] = dp[0][j-1]
        }
    }
    
    for i := 1; i <= m; i++ {
        for j := 1; j <= n; j++ {
            if p[j-1] == s[i-1] || p[j-1] == '?' {
                dp[i][j] = dp[i-1][j-1]
            } else if p[j-1] == '*' {
                dp[i][j] = dp[i][j-1] || dp[i-1][j]
            }
        }
    }
    
    return dp[m][n]
}
```

#### Complexity Analysis
- **Time Complexity**: O(m * n), where `m` is the length of the string and `n` is the length of the pattern. The 2D DP table requires computing each value once.
- **Space Complexity**: O(m * n) for storing the DP table. This can be optimized to O(n) by using only previous and current rows, if necessary.

Using dynamic programming, we significantly reduce the time complexity from exponential to polynomial time, making it scalable even for larger input sizes.

