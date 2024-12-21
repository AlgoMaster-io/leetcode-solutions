[Leetcode Problem 20: Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### Approaches
1. [Brute Force - Iterative Check](#brute-force---iterative-check)
2. [Stack Approach](#stack-approach)

---

## Brute Force - Iterative Check

### Intuition
The basic idea is to replace matching pairs of parentheses iteratively, removing them from the string until no replacements are possible. If, at the end, the string is empty, it means all brackets were paired and matched correctly.

### Approach
- Iterate over the string and look for matching pairs of `()`, `[]`, or `{}`. 
- Replace each match with an empty string.
- Repeat this process until no further replacements can be made.
- If the resultant string is empty, the original input string had balanced parentheses.

### Code
```cpp
class Solution {
public:
    bool isValid(string s) {
        // Replace matching pairs with an empty string
        while (s.find("()") != string::npos || s.find("[]") != string::npos || s.find("{}") != string::npos) {
            // Replace '()' with empty string
            size_t position = string::npos;
            while ((position = s.find("()")) != string::npos) {
                s.replace(position, 2, "");
            }
            // Replace '[]' with empty string
            while ((position = s.find("[]")) != string::npos) {
                s.replace(position, 2, "");
            }
            // Replace '{}' with empty string
            while ((position = s.find("{}")) != string::npos) {
                s.replace(position, 2, "");
            }
        }
        // If string is empty, it's valid
        return s.empty();
    }
};
```

### Time Complexity
The time complexity for this approach is O(n^2) in the worst case, where `n` is the length of the string because replacing characters in a string takes linear time complexity in C++.

### Space Complexity
The space complexity is O(1), not counting the space required to store the input string.

---

## Stack Approach

### Intuition
Use a stack to keep track of opening brackets. Whenever you encounter a closing bracket, check if it matches the top of the stack (which should be its corresponding opening bracket). This stack-based approach helps to efficiently manage matching and ensure nested/nested braces are checked properly.

### Approach
- Initialize an empty stack.
- Traverse each character in the string.
  - If the character is an opening brace `(`, `{` or `[`, push it onto the stack.
  - If it's a closing brace, check whether the stack is not empty and the top of the stack is the corresponding opening brace. If yes, pop the top of the stack. Otherwise, return false.
- At the end, if the stack is empty, then the parentheses are valid.

### Code
```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> matchStack;
        // Iterate through each character in the string
        for (char c : s) {
            // If opening bracket, push to stack
            if (c == '(' || c == '[' || c == '{') {
                matchStack.push(c);
            } else {
                // If stack is empty or mismatch in closing bracket, return false
                if (matchStack.empty() || 
                    (c == ')' && matchStack.top() != '(') ||
                    (c == ']' && matchStack.top() != '[') ||
                    (c == '}' && matchStack.top() != '{')) {
                    return false;
                }
                // Matching bracket found, pop the top
                matchStack.pop();
            }
        }
        // If stack is empty, all brackets matched, hence valid
        return matchStack.empty();
    }
};
```

### Time Complexity
The time complexity for this stack approach is O(n), where `n` is the length of the string since each character is pushed and popped from the stack at most once.

### Space Complexity
The space complexity is O(n) in the worst case for the stack, where `n` is the length of the string, representing the fully unbalanced situation like `(((((`. 

---

