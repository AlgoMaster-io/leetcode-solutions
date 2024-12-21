# [Leetcode 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches
1. [Using Stack](#using-stack)
2. [In-Place Solution](#in-place-solution)

---

## Using Stack

### Intuition
The problem requires removing adjacent duplicate characters, which can be efficiently managed using a stack data structure. The basic idea is to iterate over each character of the string and use the stack to keep track of characters while ensuring no two adjacent characters are the same. If the current character matches the character at the top of the stack, then both are adjacent duplicates, and the one at the top should be removed.

### Solution
```go
func removeDuplicates(s string) string {
    // Initialize a stack to store characters.
    stack := []rune{}

    // Iterate over each character in the string.
    for _, char := range s {
        // Check if the stack is not empty and if the top stack element equals the current character.
        if len(stack) > 0 && stack[len(stack)-1] == char {
            // Remove the top element as it's duplicate with the current character.
            stack = stack[:len(stack)-1]
        } else {
            // Push the current character onto the stack.
            stack = append(stack, char)
        }
    }

    // Convert the stack into a string by joining the runes.
    return string(stack)
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string. The algorithm iterates over the string once, and each operation on the stack (push/pop) takes constant time.
- **Space Complexity**: O(n) in the worst case, where all characters are unique, and the entire string is stored in the stack.

---

## In-Place Solution

### Intuition
The in-place solution is a more space-efficient alternative to the stack approach. Here, we modify the input string by reusing it as a stack itself. By using two pointers, one that traverses the string and the other that maintains the length of the final result, we can avoid additional space usage.

### Solution
```go
func removeDuplicates(s string) string {
    // Convert string to a slice of runes to modify it in place.
    arr := []rune(s)
    // Pointer for the end of the current "stack".
    end := 0

    // Iterate over each character.
    for _, char := range arr {
        // Check if the end pointer is non-zero and if the adjacent is a duplicate.
        if end > 0 && arr[end-1] == char {
            // Decrement end to remove the duplicate.
            end--
        } else {
            // Set the current character to the end and increment.
            arr[end] = char
            end++
        }
    }

    // Convert the rune slice back to a string.
    return string(arr[:end])
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string. The algorithm makes a single pass through the string, processing each character in constant time.
- **Space Complexity**: O(1) beyond the input storage since the algorithm modifies the input in place.

---

Both approaches efficiently solve the problem of removing adjacent duplicates, but the in-place method offers a slight optimization in terms of space usage while maintaining a similar time complexity.

