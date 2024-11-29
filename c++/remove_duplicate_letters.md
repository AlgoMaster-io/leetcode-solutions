# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approach: Monotonic Stack with Character Frequency Tracking

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1) (fixed-size stack and frequency array)
#include <stack>
#include <string>
#include <vector>

class Solution {
public:
    std::string removeDuplicateLetters(std::string s) {
        std::vector<int> lastOccurrence(26, 0); // Store last index of each character
        std::vector<bool> inStack(26, false); // Track if character is already in the stack

        // Calculate last occurrence of each character
        for (int i = 0; i < s.length(); i++) {
            lastOccurrence[s[i] - 'a'] = i;
        }

        std::stack<char> stack;

        // Iterate through the string
        for (int i = 0; i < s.length(); i++) {
            char c = s[i];

            // Skip if the character is already in the stack
            if (inStack[c - 'a']) {
                continue;
            }

            // Remove characters from the stack that are greater than the current character
            // and can appear later in the string
            while (!stack.empty() && stack.top() > c && lastOccurrence[stack.top() - 'a'] > i) {
                inStack[stack.top() - 'a'] = false;
                stack.pop();
            }

            // Push the current character onto the stack
            stack.push(c);
            inStack[c - 'a'] = true;
        }

        // Build the result string from the stack
        std::string result;
        while (!stack.empty()) {
            result = stack.top() + result;
            stack.pop();
        }

        return result;
    }
};
```


