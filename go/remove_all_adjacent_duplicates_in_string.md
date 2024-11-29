# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(n)
package main

func removeDuplicates(s string) string {
    stack := []rune{}

    // Iterate through the string
    for _, c := range s {
        // If the stack is not empty and the top element matches the current character
        if len(stack) > 0 && stack[len(stack)-1] == c {
            stack = stack[:len(stack)-1] // Remove the top element
        } else {
            stack = append(stack, c) // Add the current character to the stack
        }
    }

    return string(stack) // Convert stack back to a string
}
```

