# 921. [Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approach: Counting Mismatches

### Solution
```java
// Time Complexity: O(n), where n is the length of the string
// Space Complexity: O(1)
public class Solution {
    public int minAddToMakeValid(String s) {
        int openCount = 0; // Tracks unmatched '('
        int closeCount = 0; // Tracks unmatched ')'

        for (char c : s.toCharArray()) {
            if (c == '(') {
                openCount++; // Increment unmatched '('
            } else if (c == ')') {
                if (openCount > 0) {
                    openCount--; // Match ')' with a previous '('
                } else {
                    closeCount++; // Increment unmatched ')'
                }
            }
        }

        // Total unmatched parentheses is the sum of unmatched '(' and ')'
        return openCount + closeCount;
    }
}
```