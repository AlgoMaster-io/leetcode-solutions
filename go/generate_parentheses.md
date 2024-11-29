# 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approach 1: Brute Force (Generate All Combinations)

### Solution
go
```go
// Time Complexity: O(2^(2n) * n), where 2^(2n) is the number of combinations and n is the time to validate each combination
// Space Complexity: O(2^(2n) * n) for storing the combinations
package main

import "strings"

func generateParenthesis(n int) []string {
    var result []string
    generateAll(make([]rune, 2*n), 0, &result)
    return result
}

func generateAll(current []rune, pos int, result *[]string) {
    if pos == len(current) {
        if isValid(current) {
            *result = append(*result, string(current))
        }
        return
    }

    current[pos] = '('
    generateAll(current, pos+1, result)
    current[pos] = ')'
    generateAll(current, pos+1, result)
}

func isValid(current []rune) bool {
    balance := 0
    for _, c := range current {
        if c == '(' {
            balance++
        } else {
            balance--
        }
        if balance < 0 {
            return false
        }
    }
    return balance == 0
}
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
go
```go
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations
package main

func generateParenthesis(n int) []string {
    var result []string
    backtrack(&result, []rune{}, 0, 0, n)
    return result
}

func backtrack(result *[]string, current []rune, open, close, max int) {
    if len(current) == max*2 {
        *result = append(*result, string(current))
        return
    }

    if open < max {
        backtrack(result, append(current, '('), open+1, close, max)
    }
    if close < open {
        backtrack(result, append(current, ')'), open, close+1, max)
    }
}
```

## Approach 3: Dynamic Programming

### Solution
go
```go
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations
package main

func generateParenthesis(n int) []string {
    dp := make([][]string, n+1)
    dp[0] = []string{""}

    for i := 1; i <= n; i++ {
        var current []string
        for j := 0; j < i; j++ {
            for _, left := range dp[j] {
                for _, right := range dp[i-1-j] {
                    current = append(current, "("+left+")"+right)
                }
            }
        }
        dp[i] = current
    }

    return dp[n]
}
```

