# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approach: Monotonic Stack with Character Frequency Tracking

### Solution
```go
// Time Complexity: O(n)
// Space Complexity: O(1) (fixed-size stack and frequency array)
package main

import "fmt"

func removeDuplicateLetters(s string) string {
    lastOccurrence := make([]int, 26) // Store last index of each character
    inStack := make([]bool, 26)       // Track if character is already in the stack

    // Calculate last occurrence of each character
    for i := 0; i < len(s); i++ {
        lastOccurrence[s[i]-'a'] = i
    }

    stack := []rune{}

    // Iterate through the string
    for i := 0; i < len(s); i++ {
        c := s[i]

        // Skip if the character is already in the stack
        if inStack[c-'a'] {
            continue
        }

        // Remove characters from the stack that are greater than the current character
        // and can appear later in the string
        for len(stack) > 0 && stack[len(stack)-1] > rune(c) && lastOccurrence[stack[len(stack)-1]-'a'] > i {
            inStack[stack[len(stack)-1]-'a'] = false
            stack = stack[:len(stack)-1]
        }

        // Push the current character onto the stack
        stack = append(stack, rune(c))
        inStack[c-'a'] = true
    }

    // Build the result string from the stack
    result := string(stack)
    return result
}

func main() {
    fmt.Println(removeDuplicateLetters("bcabc")) // Output: "abc"
}
```

