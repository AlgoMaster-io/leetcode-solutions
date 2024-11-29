# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
csharp
```csharp
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public string RemoveDuplicates(string s) {
        StringBuilder stack = new StringBuilder();

        // Iterate through the string
        foreach (char c in s) {
            // If the stack is not empty and the top element matches the current character
            if (stack.Length > 0 && stack[stack.Length - 1] == c) {
                stack.Remove(stack.Length - 1, 1); // Remove the top element
            } else {
                stack.Append(c); // Add the current character to the stack
            }
        }

        return stack.ToString(); // Convert stack back to a string
    }
}
```

