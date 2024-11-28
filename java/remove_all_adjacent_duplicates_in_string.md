# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public String removeDuplicates(String s) {
        StringBuilder stack = new StringBuilder();

        // Iterate through the string
        for (char c : s.toCharArray()) {
            // If the stack is not empty and the top element matches the current character
            if (stack.length() > 0 && stack.charAt(stack.length() - 1) == c) {
                stack.deleteCharAt(stack.length() - 1); // Remove the top element
            } else {
                stack.append(c); // Add the current character to the stack
            }
        }

        return stack.toString(); // Convert stack back to a string
    }
}
```