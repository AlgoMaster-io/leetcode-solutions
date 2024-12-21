# [Leetcode 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approaches:
- [Recursive Backtracking](#recursive-backtracking)
- [Dynamic Programming](#dynamic-programming)
- [Optimized Recursive Backtracking](#optimized-recursive-backtracking)

### Recursive Backtracking

The problem of generating all combinations of well-formed parentheses can be approached using recursive backtracking. The idea is to build the string step by step, placing '(' when we can still add left parentheses and ')' when we can add right parentheses without breaking the balance.

**Intuition:**
- Use backtracking to try adding parentheses.
- Use a recursive function to add either '(' or ')' at each step.
- Ensure that at no point should the number of ')' exceed the number of '(' to maintain balance.

#### Algorithm:
1. Start with an empty string and zero depth.
2. Add '(' if it's possible, and recurse.
3. Add ')' only if it keeps the sequence valid.
4. If the string reaches a length of `2 * n`, where `n` is the number of pairs, store it.

#### Complexity:
- Time Complexity: O(4^n / √n), Catalan number Cn.
- Space Complexity: O(n)

```go
func generateParenthesis(n int) []string {
    var result []string
    var backtrack func(current string, open, close int)

    backtrack = func(current string, open, close int) {
        // If current string has reached the maximum length, add to result
        if len(current) == 2*n {
            result = append(result, current)
            return
        }
        // If we can add an open '(', do so
        if open < n {
            backtrack(current+"(", open+1, close)
        }
        // If we can add a close ')', do so
        if close < open {
            backtrack(current+")", open, close+1)
        }
    }

    backtrack("", 0, 0)
    return result
}
```

### Dynamic Programming

Dynamic Programming can be used to compute all combinations by building from already known results of smaller subproblems.

**Intuition:**
- Use the combinatorics of parentheses to create a dynamic programming formula.
- If a valid sequence is formed of sub-sequences, consider combining sub-results.

#### Algorithm:
1. Start with base case: A valid sequence with zero pairs.
2. Use previous results to generate new results.

#### Complexity:
- Time Complexity: O(4^n / √n)
- Space Complexity: O(4^n / √n)

```go
func generateParenthesis(n int) []string {
    dp := make([][]string, n+1)
    dp[0] = []string{""}

    for i := 1; i <= n; i++ {
        var current []string
        for j := 0; j < i; j++ {
            for _, left := range dp[j] {
                for _, right := range dp[i-j-1] {
                    current = append(current, "("+left+")"+right)
                }
            }
        }
        dp[i] = current
    }
    
    return dp[n]
}
```

### Optimized Recursive Backtracking

This approach improves upon the basic recursive backtracking by eliminating unnecessary function calls using efficient pruning strategies.

**Intuition:**
- Similar to recursive backtracking but adds checks to ensure we don't explore invalid states.

#### Algorithm:
1. Same as recursive, but avoid states that can never lead to valid solutions early on.
2. Use logical checks to prune unnecessary branches.
3. Efficient track and build solutions.

#### Complexity:
- Time Complexity: O(4^n / √n)
- Space Complexity: O(n)

```go
func generateParenthesis(n int) []string {
    res := []string{}
    var backtrack func(S string, left, right int)
    backtrack = func(S string, left, right int) {
        // Base case: If the string is complete
        if len(S) == n*2 {
            res = append(res, S)
            return
        }
        // Add an opening bracket if possible
        if left < n {
            backtrack(S+"(", left+1, right)
        }
        // Add a closing bracket if possible
        if right < left {
            backtrack(S+")", left, right+1)
        }
    }

    backtrack("", 0, 0)
    return res
}
```

In this document, the problem is approached with increasing levels of sophistication and optimization to efficiently generate all combinations of valid parentheses. Each method showcases its unique approach, highlighting the fundamental concepts of recursion, backtracking, dynamic programming, and optimization strategies in algorithm design.

