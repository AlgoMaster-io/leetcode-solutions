# [Leetcode 516: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Approaches:
- [Approach 1: Recursive Solution](#approach-1-recursive-solution)
- [Approach 2: Recursive with Memoization](#approach-2-recursive-with-memoization)
- [Approach 3: Dynamic Programming](#approach-3-dynamic-programming)

## Approach 1: Recursive Solution

### Intuition:
The problem can be broken down into a recursive problem by considering every possible way to form a palindrome from a given string. For a string `s`, a recursive relation can be developed based on whether the characters at both ends are equal or not. If they are equal, they form part of the palindrome.

### Approach:
1. Start with the first and last character of the string.
2. If they are equal, they form a part of the palindrome; solve the subproblem by moving inward.
3. If they are not equal, solve two subproblems: one excluding the first character and one excluding the last character, and return the maximum of the two results.
4. This will give us all possible palindromic subsequences, and we can keep track of the longest one.

```go
func longestPalindromeSubseq(s string) int {
    return longestPalindromeRec(s, 0, len(s)-1)
}

func longestPalindromeRec(s string, i int, j int) int {
    if i > j {
        return 0
    }
    if i == j {
        return 1
    }
    if s[i] == s[j] {
        return 2 + longestPalindromeRec(s, i+1, j-1)
    } else {
        return max(longestPalindromeRec(s, i+1, j), longestPalindromeRec(s, i, j-1))
    }
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

**Time Complexity:** `O(2^n)` - Recursive without any memoization leads to an exponential time complexity.

**Space Complexity:** `O(n)` - Call stack space for recursion.

---

## Approach 2: Recursive with Memoization

### Intuition:
To improve upon the basic recursive solution, recognize that there are overlapping subproblems. By storing the results of these subproblems, unnecessary recalculations can be avoided.

### Approach:
1. Use a 2D slice (matrix) to store previously computed results of subproblems.
2. Follow the recursive logic from Approach 1 but before computing, check if the result is already in the 2D slice.
3. Use this stored result if available, otherwise compute and store it.

```go
func longestPalindromeSubseq(s string) int {
    n := len(s)
    memo := make([][]int, n)
    for i := range memo {
        memo[i] = make([]int, n)
    }
    return longestPalindromeMemo(s, 0, n-1, memo)
}

func longestPalindromeMemo(s string, i, j int, memo [][]int) int {
    if i > j {
        return 0
    }
    if i == j {
        return 1
    }
    if memo[i][j] != 0 {
        return memo[i][j]
    }
    if s[i] == s[j] {
        memo[i][j] = 2 + longestPalindromeMemo(s, i+1, j-1, memo)
    } else {
        memo[i][j] = max(longestPalindromeMemo(s, i+1, j, memo), longestPalindromeMemo(s, i, j-1, memo))
    }
    return memo[i][j]
}
```

**Time Complexity:** `O(n^2)` - Each subproblem is computed once.

**Space Complexity:** `O(n^2)` - 2D slice to store results of subproblems.

---

## Approach 3: Dynamic Programming

### Intuition:
The recursive with memoization approach can be transformed into a bottom-up dynamic programming solution, leveraging an iterative method to fill out a DP table, considering all substrings.

### Approach:
1. Create a 2D DP table where `dp[i][j]` will store the length of the longest palindromic subsequence in substring `s[i...j]`.
2. Initialize the table with each single character as a palindrome of length 1 (i.e., all `dp[i][i] = 1`).
3. Fill the table for all possible lengths starting from 2 to n.
4. Within the loop, if characters at `i` and `j` are equal, then the length is `2 + dp[i+1][j-1]`, else it is `max(dp[i+1][j], dp[i][j-1])`.

```go
func longestPalindromeSubseq(s string) int {
    n := len(s)
    dp := make([][]int, n)
    for i := range dp {
        dp[i] = make([]int, n)
    }
    
    // Each character is a palindrome of length 1
    for i := 0; i < n; i++ {
        dp[i][i] = 1
    }

    // Fill the table
    for length := 2; length <= n; length++ {
        for i := 0; i <= n-length; i++ {
            j := i + length - 1
            if s[i] == s[j] {
                dp[i][j] = 2 + dp[i+1][j-1]
            } else {
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])
            }
        }
    }

    // The answer is in the whole string
    return dp[0][n-1]
}
```

**Time Complexity:** `O(n^2)` - We fill up a 2D table for all substrings.

**Space Complexity:** `O(n^2)` - Space to store the DP table.

