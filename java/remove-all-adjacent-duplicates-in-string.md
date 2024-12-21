[Leetcode 1047: Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approaches
1. [Using a Stack](#approach-1-using-a-stack)

---

### Approach 1: Using a Stack

**Intuition**

The problem requires removal of adjacent duplicates in the given string `s`. This can be efficiently solved using a stack. The basic idea is to iterate over each character of the string, and use a stack to keep track of characters, ensuring that whenever two consecutive characters are the same, we pop them from the stack, effectively removing those duplicates.

**Algorithm** 
1. Initialize an empty stack to store characters.
2. Iterate through each character `c` in the string `s`.
3. For each character, check if the stack is not empty and the top of the stack is equal to `c`.
   - If true, pop the top element (this means we've found a duplicate).
   - Else, push the current character `c` onto the stack.
4. Finally, construct the resulting string from the characters left in the stack.

**Java Code**
```java
public class Solution {
    public String removeDuplicates(String s) {
        // Use a StringBuilder as a stack to keep track of characters
        StringBuilder stack = new StringBuilder();

        // Iterate over each character in the input string
        for (char c : s.toCharArray()) {
            // If the stack is not empty and the last character in the stack is the same as the current one
            if (stack.length() > 0 && stack.charAt(stack.length() - 1) == c) {
                // Pop the character from the stack
                stack.deleteCharAt(stack.length() - 1);
            } else {
                // Push the current character to the stack
                stack.append(c);
            }
        }

        // Return the remaining characters as the result string
        return stack.toString();
    }
}
```

**Time Complexity**: O(n), where n is the length of the string `s`. Each character is pushed and popped from the stack at most once.

**Space Complexity**: O(n) in the worst case where there are no adjacent duplicates, and the stack contains all characters of the string.

