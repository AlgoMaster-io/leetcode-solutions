# [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

## Table of Contents
1. [Approach 1: Brute Force](#approach-1-brute-force)
2. [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
3. [Approach 3: Using Stack](#approach-3-using-stack)
4. [Approach 4: Two Pass Scan](#approach-4-two-pass-scan)

---

## Approach 1: Brute Force

The brute force approach involves checking every possible substring to see if it forms a valid parentheses string, then tracking the maximum length.

### Intuition:
- Iterate over every possible pair of starting and ending indices.
- For each substring defined by these indices, check if it's a valid parentheses string.
- A valid parentheses substring has matching open and close brackets.
- Maintain a counter for valid matching and if it equals the length of the substring, update the max length.

### Code:
```go
func isValid(s string) bool {
    balance := 0
    for i := 0; i < len(s); i++ {
        if s[i] == '(' {
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

func longestValidParentheses(s string) int {
    maxLength := 0
    for i := 0; i < len(s); i++ {
        for j := i + 2; j <= len(s); j += 2 {
            if isValid(s[i:j]) {
                maxLength = max(maxLength, j-i)
            }
        }
    }
    return maxLength
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}
```

### Complexity Analysis:
- **Time Complexity**: O(n^3), due to the nested loop structure and the validation performed for each substring.
- **Space Complexity**: O(1), excluding the input space.

---

## Approach 2: Dynamic Programming

We use dynamic programming to solve this problem efficiently by maintaining a `dp` array.

### Intuition:
- Use `dp[i]` to represent the length of the longest valid parentheses ending at index `i`.
- Iterate over the string.
  - If `s[i]` is ')', then check:
    - If `s[i-1]` is '(', then `dp[i] = dp[i-2] + 2`.
    - If `s[i-1]` is ')' and `s[i-dp[i-1]-1]` is '(', then `dp[i] = dp[i-1] + dp[i-dp[i-1]-2] + 2`.

### Code:
```go
func longestValidParentheses(s string) int {
    n := len(s)
    if n == 0 {
        return 0
    }

    dp := make([]int, n)
    maxLength := 0

    for i := 1; i < n; i++ {
        if s[i] == ')' {
            if s[i-1] == '(' {
                dp[i] = (i >= 2 ? dp[i-2] : 0) + 2
            } else if i-dp[i-1] > 0 && s[i-dp[i-1]-1] == '(' {
                dp[i] = dp[i-1] + (i-dp[i-1] >= 2 ? dp[i-dp[i-1]-2] : 0) + 2
            }
            if dp[i] > maxLength {
                maxLength = dp[i]
            }
        }
    }

    return maxLength
}
```

### Complexity Analysis:
- **Time Complexity**: O(n), single pass through the string.
- **Space Complexity**: O(n), for the dp array.

---

## Approach 3: Using Stack

The stack approach maintains indices of unbalanced '(' and potential starting positions of valid substrings.

### Intuition:
- Use a stack to track indices of parentheses.
- Initialize stack with `-1` to support edge use cases.
- For every ')' matched with an index from stack, compute the length of valid substring.
- This ensures the balance of parentheses is maintained.

### Code:
```go
func longestValidParentheses(s string) int {
    maxLength := 0
    stack := []int{-1}

    for i := 0; i < len(s); i++ {
        if s[i] == '(' {
            stack = append(stack, i)
        } else {
            stack = stack[:len(stack)-1]
            if len(stack) == 0 {
                stack = append(stack, i)
            } else {
                currentLength := i - stack[len(stack)-1]
                maxLength = max(maxLength, currentLength)
            }
        }
    }

    return maxLength
}
```

### Complexity Analysis:
- **Time Complexity**: O(n), processing each character once.
- **Space Complexity**: O(n), for the stack.

---

## Approach 4: Two Pass Scan

Two-pass scan involves scanning the string forwards and backwards to maintain the balance of parentheses.

### Intuition:
- First pass (left to right): Count '('s and ')'s. Reset if invalid.
- Second pass (right to left): Count ')'s and '('s. Reset if invalid.
- Maximum length found in both scans gives the answer.

### Code:
```go
func longestValidParentheses(s string) int {
    left, right, maxLength := 0, 0, 0

    // Left to Right Scan
    for i := 0; i < len(s); i++ {
        if s[i] == '(' {
            left++
        } else {
            right++
        }
        if left == right {
            maxLength = max(maxLength, 2*right)
        } else if right > left {
            left, right = 0, 0
        }
    }

    left, right = 0, 0

    // Right to Left Scan
    for i := len(s) - 1; i >= 0; i-- {
        if s[i] == ')' {
            right++
        } else {
            left++
        }
        if left == right {
            maxLength = max(maxLength, 2*left)
        } else if left > right {
            left, right = 0, 0
        }
    }

    return maxLength
}
```

### Complexity Analysis:
- **Time Complexity**: O(n), two linear scans of the string.
- **Space Complexity**: O(1), no additional space is used beyond variables.

