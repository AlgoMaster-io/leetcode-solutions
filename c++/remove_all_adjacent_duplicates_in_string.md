# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    string removeDuplicates(string s) {
        string stack;

        // Iterate through the string
        for (char c : s) {
            // If the stack is not empty and the top element matches the current character
            if (!stack.empty() && stack.back() == c) {
                stack.pop_back(); // Remove the top element
            } else {
                stack.push_back(c); // Add the current character to the stack
            }
        }

        return stack; // Return the stack as a string
    }
};
```

