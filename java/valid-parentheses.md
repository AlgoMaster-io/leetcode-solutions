# [Leetcode Problem 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approaches:
- [Approach 1: Stack Data Structure](#approach-1-stack-data-structure)

## Approach 1: Stack Data Structure

### Intuition
The problem of validating parentheses can be efficiently solved using a stack data structure as it is ideal for problems involving matched pairs and nested structures. The idea here is to iterate through the characters of the string and use the stack to keep track of opening parentheses. When we encounter a closing parenthesis, we check if it matches the top of the stack. If it does, we can pop the stack; otherwise, the string is invalid.

### Detailed Steps
1. Initialize a stack to keep track of opening parentheses.
2. Loop through each character in the string.
3. If a character is an opening bracket ('(', '{', or '['), push it onto the stack.
4. If it is a closing bracket, check the stack:
   - If the stack is empty, it means there is no matching opening bracket, thus the string is invalid.
   - If the stack is not empty, pop the top element from the stack and check if it matches the closing bracket. If not, return false.
5. After processing all characters, if the stack is empty, it means all opening brackets had matching closing brackets, so the string is valid. Otherwise, return false.

### Java Code
```java
import java.util.Stack;

public class Solution {
    public boolean isValid(String s) {
        // Initialize a stack to store opening brackets
        Stack<Character> stack = new Stack<>();
        
        // Loop over each character in the string
        for (char c : s.toCharArray()) {
            // If it's an opening bracket, push onto stack
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else {
                // If stack is empty, there's no matching opening bracket
                if (stack.isEmpty()) return false;
                
                // Pop the top element to check for matching opening bracket
                char top = stack.pop();
                // Check if the bracket types do not match
                if (c == ')' && top != '(') return false;
                if (c == '}' && top != '{') return false;
                if (c == ']' && top != '[') return false;
            }
        }
        
        // Return true if stack is empty, indicating all opening brackets had closing matches
        return stack.isEmpty();
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n), where n is the length of the string. We traverse the string once, and each operation on the stack (push/pop) is O(1).
- **Space Complexity**: O(n) in the worst case, when all characters are opening brackets and are pushed onto the stack.

