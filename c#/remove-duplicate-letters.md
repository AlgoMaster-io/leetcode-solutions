# [Leetcode 316: Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approaches

1. [Greedy with Stack](#greedy-with-stack)

---

## 1. Greedy with Stack

### Intuition
The problem requires us to return the smallest lexicographical order string possible after removing duplicate letters. We need to make sure that each character appears in the result string exactly once and the result string is in the smallest lexicographical order. A stack can efficiently help keep track of the characters being added to the resulting string by enabling easy removal of characters when a better (smaller lexicographical order) character is encountered.

### Steps
1. Count occurrences of each character.
2. Use a boolean array to keep track of which characters are in the current result (simulating a set).
3. Initialize a stack to build the resulting string.
4. Iterate over each character in the input string:
   - If the character is already in the result, skip it and decrement its count.
   - While the stack is not empty and the current character is lexicographically smaller than the top of the stack and the top character can appear later, pop the character from the stack.
   - Add the current character to the stack and mark it as present in the result.
5. Convert the stack into a string for the result.

### Code
```csharp
public class Solution {
    public string RemoveDuplicateLetters(string s) {
        // Step 1: Count occurrences of each character
        int[] charCount = new int[26];
        foreach (char c in s) {
            charCount[c - 'a']++;
        }

        // Boolean array to track characters currently in stack/result
        bool[] inStack = new bool[26];

        // Initialize a stack to build the result
        Stack<char> stack = new Stack<char>();

        // Step 4: Iterate over each character in the input string
        foreach (char c in s) {
            // Decrease the count of the current character
            charCount[c - 'a']--;

            // If character is already in result, skip it
            if (inStack[c - 'a']) {
                continue;
            }

            // Remove characters from stack if conditions are met
            while (stack.Count > 0 && stack.Peek() > c && charCount[stack.Peek() - 'a'] > 0) {
                char removed = stack.Pop();
                inStack[removed - 'a'] = false;
            }

            // Add current character to stack and mark it as in result
            stack.Push(c);
            inStack[c - 'a'] = true;
        }

        // Return the stack as a string (result)
        char[] result = stack.Reverse().ToArray();
        return new string(result);
    }
}
```

### Time Complexity
- **O(N)**, where N is the length of the input string `s`. The algorithm processes each character a constant number of times.

### Space Complexity
- **O(1)**, only a fixed amount of additional space is used (arrays and stack), relative to the input size since the size of used arrays and stack are bounded by the constant number of characters (26 lowercase English letters).

This approach effectively uses a greedy method with a stack to ensure that we maintain the smallest lexicographical order and all characters appear exactly once in the final result.

