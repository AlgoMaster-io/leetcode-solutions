# [Leetcode 316: Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approaches

1. [Greedy Approach with Stack](#greedy-approach-with-stack)

---

### Greedy Approach with Stack

**Intuition:**

The goal is to return the lexicographically smallest string possible by removing the duplicate letters and keeping the first occurrence optimal.

To achieve this, we can utilize a greedy approach supplemented by a stack. The stack is used to build the result character by character while ensuring that:

- Each character occurs only once.
- The resulting string is the smallest possible in lexicographical order.

The key points to apply this method involve:
- A counter to determine how many characters are left for each unique character in the string.
- A stack to help in constructing our answer which guarantees the lexicographical order.
- A boolean array or set to keep track of characters that are already in the result stack.

**Steps:**

1. Maintain a `count` array to store the frequency of each character.
2. Use a stack to build the final string and a `seen` boolean array to check if a character is added to the result.
3. Traverse the input string character by character:
   - Decrement its count as it's traversed.
   - If the character is already in the stack, continue to next.
   - If the current character is smaller than the top character of the stack and the top character appears later in the string (based on count array), pop the stack.
   - Add the current character to the stack and mark it as seen.

4. Convert the stack to a string to construct the result.

```go
func removeDuplicateLetters(s string) string {
    // Count frequency of each character in the string
    count := make(map[rune]int)
    for _, char := range s {
        count[char]++
    }

    // Stack to construct the final result
    stack := []rune{}
    // Boolean map to track the presence of character in stack
    seen := make(map[rune]bool)

    for _, char := range s {
        // Decrement count for the current character
        count[char]--

        // If character already in result, skip it
        if seen[char] {
            continue
        }

        // Maintain the order, pop characters from the stack if they're greater than current char and can occur later
        for len(stack) > 0 && stack[len(stack)-1] > char && count[stack[len(stack)-1]] > 0 {
            top := stack[len(stack)-1]  // Get the last element in stack
            stack = stack[:len(stack)-1] // Pop the last element
            seen[top] = false           // Mark it as not seen
        }

        // Add current character to stack and mark as seen
        stack = append(stack, char)
        seen[char] = true
    }

    // Build final result from stack
    return string(stack)
}
```

**Time Complexity:** O(n) - where n is the length of the string, as each character is processed at most twice (once pushed and once popped in any case).

**Space Complexity:** O(1) - We use a count and seen map with the size proportional to the character set which is limited. The stack stores each character of the input string once.

