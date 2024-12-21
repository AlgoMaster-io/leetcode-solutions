# [Leetcode 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Solutions:
- [Backtracking Approach](#backtracking-approach)

### Backtracking Approach

The problem of generating valid parentheses can be approached using a backtracking technique. The key is to maintain a balance between open and close parentheses while exploring all possible sequences.

**Intuition:**

The idea is to build the solution step by step and undo the incorrect decision. We will start with an empty string and try to add either an open '(' or a close ')' parenthesis until we have constructed a valid combination of parentheses. 

Rules for adding parentheses:
1. Add an open parenthesis '(' if we still have an open one left to place.
2. Add a close parenthesis ')' if it would not lead to an invalid sequence, e.g., we can add ')' only if the number of open ones is greater than the number of close ones used.

This approach will help us generate all balanced parentheses by ensuring at every point that the sequence is valid.

#### Code:

```javascript
var generateParenthesis = function(n) {
    const result = [];

    // Recursive function to generate the parentheses combinations
    const backtrack = (currentString, openCount, closeCount) => {
        
        // If the current string is valid and has reached the maximum length
        if (currentString.length === n * 2) {
            result.push(currentString);
            return;
        }

        // If we can still place an open parenthesis
        if (openCount < n) {
            // Try adding an open parenthesis
            backtrack(currentString + '(', openCount + 1, closeCount);
        }

        // If we can place a close parenthesis without invalidating the string
        if (closeCount < openCount) {
            // Try adding a close parenthesis
            backtrack(currentString + ')', openCount, closeCount + 1);
        }
    };

    // Initialize the backtracking with an empty string and 0 count of open and close parenthesis
    backtrack("", 0, 0);
    return result;
};
```

#### Complexity Analysis:
- **Time Complexity:** O(4^n / √n)
  - Each valid sequence has a well-defined length, and the Catalan number grows exponentially with `n`.
- **Space Complexity:** O(n)
  - The recursion stack will go at most `n` levels deep. Additionally, result storage grows with the number of valid sequences generated.

This method efficiently generates all possible parenthesis combinations using the rules defined and ensures that only valid parentheses are formed by the end of recursion calls. Each call either adds a parenthesis or backtracks after a choice, making it a fitting candidate for backtracking.

