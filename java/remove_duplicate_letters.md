# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approach: Monotonic Stack with Character Frequency Tracking

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1) (fixed-size stack and frequency array)
import java.util.Stack;

public class Solution {
    public String removeDuplicateLetters(String s) {
        int[] lastOccurrence = new int[26]; // Store last index of each character
        boolean[] inStack = new boolean[26]; // Track if character is already in the stack

        // Calculate last occurrence of each character
        for (int i = 0; i < s.length(); i++) {
            lastOccurrence[s.charAt(i) - 'a'] = i;
        }

        Stack<Character> stack = new Stack<>();

        // Iterate through the string
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);

            // Skip if the character is already in the stack
            if (inStack[c - 'a']) {
                continue;
            }

            // Remove characters from the stack that are greater than the current character
            // and can appear later in the string
            while (!stack.isEmpty() && stack.peek() > c && lastOccurrence[stack.peek() - 'a'] > i) {
                inStack[stack.pop() - 'a'] = false;
            }

            // Push the current character onto the stack
            stack.push(c);
            inStack[c - 'a'] = true;
        }

        // Build the result string from the stack
        StringBuilder result = new StringBuilder();
        for (char c : stack) {
            result.append(c);
        }

        return result.toString();
    }
}
```