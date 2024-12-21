# [Leetcode 921: Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approaches
1. [Stack-Based Solution](#stack-based-solution)
2. [Balance-based solution](#balance-based-solution)

### Stack-Based Solution

The stack-based approach leverages the Last-In-First-Out (LIFO) principle to maintain a valid set of parentheses. Here's the step-by-step intuition:

- Use a stack to keep track of unmatched opening parentheses '('.
- Traverse the string character by character.
- If you encounter an opening parenthesis '(', push it onto the stack.
- If you encounter a closing parenthesis ')':
  - If the stack is not empty, pop an element from the stack (indicating a match).
  - If the stack is empty, increment a counter for unmatched closing parentheses.
- At the end, the size of the stack gives the count of unmatched opening parentheses, and the counter gives unmatched closing parentheses. Their sum is the minimum number of additions you need.

```go
func minAddToMakeValid(S string) int {
    // Stack to keep track of unmatched '('
    stack := []rune{}
    // Counter for unmatched ')'
    unmatchedClosing := 0

    for _, char := range S {
        if char == '(' {
            // Push '(' onto stack
            stack = append(stack, char)
        } else if char == ')' {
            if len(stack) > 0 {
                // Pop '(' from stack if available, matched with ')'
                stack = stack[:len(stack)-1]
            } else {
                // Increment unmatched ')' counter
                unmatchedClosing++
            }
        }
    }
    
    // Stack length is unmatched '(', unmatchedClosing is unmatched ')'
    return len(stack) + unmatchedClosing
}
```

- **Time Complexity:** O(n) where n is the length of the string, because we iterate through the string once.
- **Space Complexity:** O(n) in the worst case if the string contains only opening parentheses.

### Balance-Based Solution

This approach utilizes a balance counter to maintain the count of parentheses:

- Balance counter: A simulated stack that tracks the balance between opening and closing parentheses.
- For each character:
  - Increment the balance if it's '('.
  - Decrement the balance if it's ')'.
    - If balance becomes negative (more ')' than '('), it indicates the need for an extra '(' to balance. We increment a counter to track these excess parentheses and reset balance to zero.
- The final answer is the sum of the unbalanced openings (balance counter) and the excess counter.

```go
func minAddToMakeValid(S string) int {
    balance := 0
    unbalancedOpenings := 0

    for _, char := range S {
        if char == '(' {
            // Increment balance for each '('
            balance++
        } else if char == ')' {
            // Decrement balance for each ')'
            balance--
            // Check if we have excess closing parenthesis
            if balance < 0 {
                // Increment unbalancedOpenings as we have an excess ')'
                unbalancedOpenings++
                balance = 0
            }
        }
    }
    
    // Return the sum of unbalanced '(', and excess ')'
    return balance + unbalancedOpenings
}
```

- **Time Complexity:** O(n), where n is the length of the string.
- **Space Complexity:** O(1), since we only use a few counters and no auxiliary data structures like a stack. This makes it optimal in terms of space.

