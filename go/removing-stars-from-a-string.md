# [Leetcode 2390: Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)

## Approaches
- [Approach 1: Stack Simulation](#approach-1-stack-simulation)
- [Approach 2: Two-Pointer Approach](#approach-2-two-pointer-approach)

---

## Approach 1: Stack Simulation

### Intuition
The problem can efficiently be solved using a stack simulation where we iterate over the string and simulate the stack operations manually. The basic idea is to iterate over each character in the string:
- If the character is not a '*', we push it onto the stack.
- If the character is a '*', it means we should remove the character just before it, which means popping from the stack.
This approach takes advantage of the last-in-first-out (LIFO) nature of the stack to efficiently manage removal of characters as directed by the '*' characters.

### Go Code

```go
func removeStars(s string) string {
    // Initialize a stack represented by a slice
    stack := []rune{}
    
    // Iterate over each character in the string
    for _, ch := range s {
        if ch == '*' {
            // Pop the last character from the stack if it is not empty
            if len(stack) > 0 {
                stack = stack[:len(stack)-1]
            }
        } else {
            // Push the current character onto the stack
            stack = append(stack, ch)
        }
    }
    
    // Convert the stack to a string to get the final result
    return string(stack)
}
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the string `s`. Each character is processed once.
- **Space Complexity:** O(n), in the worst case, the stack can store every character from the input string except stars.

## Approach 2: Two-Pointer Approach

### Intuition
This approach optimizes space usage by using two pointers: one pointer to traverse the string and another to manage valid characters in the input directly without explicitly using a stack. The idea is to maintain a 'write' pointer that reflects the processed-valid part of the string:
- Traverse the string with a read pointer.
- If the current character is not '*', place it at the current write position and increment the write pointer.
- If the current character is '*', decrement the write pointer indicating removal of the last valid character.

### Go Code

```go
func removeStars(s string) string {
    // Convert the string to a slice of runes for in-place operations
    result := []rune(s)
    writePointer := 0
    
    // Iterate over each character in the string using a readPointer
    for _, ch := range s {
        if ch == '*' {
            // 'Remove' the last valid character by moving the write pointer back
            writePointer--
        } else {
            // Place the character at the current write pointer position
            result[writePointer] = ch
            writePointer++
        }
    }
    
    // Convert the result to a string using only valid characters
    return string(result[:writePointer])
}
```

### Complexity Analysis
- **Time Complexity:** O(n), where n is the length of the string `s`. The entire string is traversed once with constant time operations.
- **Space Complexity:** O(1), since the operations are performed in-place on the input string. Only a constant amount of extra space is used.

