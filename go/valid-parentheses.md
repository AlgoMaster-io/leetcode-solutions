### [LeetCode Problem 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

#### Approaches:
- [Approach 1: Brute Force with Stack](#approach-1-brute-force-with-stack)
- [Approach 2: Optimized Stack Approach](#approach-2-optimized-stack-approach)

---

### Approach 1: Brute Force with Stack

**Intuition:**

The problem here is to determine whether the given string consisting only of parentheses is valid. A valid string requires every opening bracket to be matched with a corresponding closing bracket in the correct order. By using a stack, a Last In, First Out (LIFO) data structure, we can effectively pair the parentheses.

**Detailed Steps:**

1. Create a stack to store opening brackets.
2. Iterate through each character in the string:
   - If the character is an opening bracket, push it onto the stack.
   - If the character is a closing bracket, check if the stack is not empty and if the top of the stack has the corresponding opening bracket. If it does, pop the opening bracket from the stack. If not, return false.
3. After processing all characters, if the stack is empty, all opening brackets were matched correctly, and the string is valid. Otherwise, return false.

**Code:**

```go
func isValid(s string) bool {
    // Initializing stack as a slice
    stack := []rune{}
    // Mapping of opening brackets to their corresponding closing brackets
    bracketMap := map[rune]rune{
        ')': '(',
        '}': '{',
        ']': '[',
    }

    // Iterate over each character in the string
    for _, char := range s {
        // Check if character is a closing bracket
        if open, exists := bracketMap[char]; exists {
            // If stack is non-empty and top of the stack matches the corresponding opening bracket
            if len(stack) > 0 && stack[len(stack)-1] == open {
                stack = stack[:len(stack)-1] // pop the top element
            } else {
                return false // mismatch or stack is empty when a closing bracket appears
            }
        } else {
            // Push opening bracket into the stack
            stack = append(stack, char)
        }
    }
    // If stack is empty, all opening brackets were matched
    return len(stack) == 0
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the length of the string. We process each character once.
- **Space Complexity:** O(n) in the worst case, for example `(((((((((()))))))),` when the stack contains all opening brackets.

---

### Approach 2: Optimized Stack Approach

**Intuition:**

Instead of storing the actual opening brackets in the stack, we can optimize by only storing their expected closing counterparts. This would make the stack comparison slightly more efficient by eliminating the need for an additional lookup on each pop operation.

**Detailed Steps:**

1. Utilize a stack to keep track of expected closing brackets.
2. Iterate through each character in the string:
   - If it is an opening bracket, push the corresponding closing bracket onto the stack.
   - If it is a closing bracket and it matches the top element of the stack, pop the stack.
   - If it doesn't match or the stack is empty, the string is invalid.
3. If the stack is empty after processing all characters, the string is valid.

**Code:**

```go
func isValidOptimized(s string) bool {
    // Stack of expected closing brackets
    stack := []rune{}

    // Iterate over each character in the string
    for _, char := range s {
        switch char {
        // If opening bracket, push expected closing bracket onto stack
        case '(':
            stack = append(stack, ')')
        case '{':
            stack = append(stack, '}')
        case '[':
            stack = append(stack, ']')
        default:
            // Default case for closing brackets
            if len(stack) == 0 || stack[len(stack)-1] != char {
                return false
            }
            // Pop the top of the stack since it is a matched pair
            stack = stack[:len(stack)-1]
        }
    }
    // If stack is empty, return true, meaning all opening brackets matched
    return len(stack) == 0
}
```

**Complexity Analysis:**

- **Time Complexity:** O(n), where n is the length of the string.
- **Space Complexity:** O(n) in the worst case, when all characters are opening brackets.

By minimizing the operations performed in the loop, this approach can be slightly more efficient in practice, while maintaining the same asymptotic complexity.

