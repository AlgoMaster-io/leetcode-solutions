# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

import "fmt"

func isValid(s string) bool {
    stack := []rune{}

    // Iterate through the string
    for _, c := range s {
        // Push opening brackets onto the stack
        if c == '(' || c == '{' || c == '[' {
            stack = append(stack, c)
        } else {
            // If stack is empty or doesn't match the top, return false
            if len(stack) == 0 || !isMatchingPair(stack[len(stack)-1], c) {
                return false
            }
            // Pop the top element from the stack
            stack = stack[:len(stack)-1]
        }
    }

    // If the stack is empty, all brackets are matched
    return len(stack) == 0
}

func isMatchingPair(open, close rune) bool {
    return (open == '(' && close == ')') ||
           (open == '{' && close == '}') ||
           (open == '[' && close == ']')
}
```

