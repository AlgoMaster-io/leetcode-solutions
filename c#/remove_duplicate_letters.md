# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approach: Monotonic Stack with Character Frequency Tracking

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(1) (fixed-size stack and frequency array)
using System.Collections.Generic;
using System.Text;

public class Solution {
    public string RemoveDuplicateLetters(string s) {
        int[] lastOccurrence = new int[26]; // Store last index of each character
        bool[] inStack = new bool[26]; // Track if character is already in the stack

        // Calculate last occurrence of each character
        for (int i = 0; i < s.Length; i++) {
            lastOccurrence[s[i] - 'a'] = i;
        }

        Stack<char> stack = new Stack<char>();

        // Iterate through the string
        for (int i = 0; i < s.Length; i++) {
            char c = s[i];

            // Skip if the character is already in the stack
            if (inStack[c - 'a']) {
                continue;
            }

            // Remove characters from the stack that are greater than the current character
            // and can appear later in the string
            while (stack.Count > 0 && stack.Peek() > c && lastOccurrence[stack.Peek() - 'a'] > i) {
                inStack[stack.Pop() - 'a'] = false;
            }

            // Push the current character onto the stack
            stack.Push(c);
            inStack[c - 'a'] = true;
        }

        // Build the result string from the stack
        StringBuilder result = new StringBuilder();
        foreach (char c in stack) {
            result.Append(c);
        }

        return result.ToString();
    }
}
```

