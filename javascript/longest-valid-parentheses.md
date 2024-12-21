# [Leetcode 32: Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

## Table of Contents
1. [Stack-based Approach](#stack-based-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Two-Pass Counter Approach](#two-pass-counter-approach)

---

## Stack-based Approach

### Intuition:

The stack-based approach leverages the ability to match parentheses effectively. By using a stack, we can track open parentheses indices and ensure we keep track of valid closed parentheses. The stack helps determine the boundaries of valid parentheses by matching pairs and tracking indices accurately.

1. Initialize a stack and push the base index `-1` to handle edge cases.
2. Iterate over each character in the string:
    - If it's an open parenthesis `(`, push its index onto the stack.
    - If it's a closed parenthesis `)`:
        - Pop the top of the stack. If the stack is empty after popping, push the current index onto the stack (indicating an unmatched closing).
        - Otherwise, calculate the length of the current valid substring by subtracting the current index from the top of the stack to keep the longest valid sequence length updated.
3. Return the maximum valid length.

### Code:

```javascript
function longestValidParentheses(s) {
    let maxLength = 0;
    let stack = [-1]; // Initialize stack with base index

    for (let i = 0; i < s.length; i++) {
        if (s[i] === '(') {
            stack.push(i); // Push index of '(' onto the stack
        } else {
            stack.pop(); // Pop the last open parenthesis index

            if (stack.length === 0) {
                stack.push(i); // Base case for unmatched ')'
            } else {
                maxLength = Math.max(maxLength, i - stack[stack.length - 1]); // Calculate valid length
            }
        }
    }

    return maxLength;
}
```

### Time Complexity:

- O(n), where n is the length of the string, as we're iterating over the string once.

### Space Complexity:

- O(n), in the worst case, we push all indices onto the stack.

---

## Dynamic Programming Approach

### Intuition:

The dynamic programming approach utilizes an array to store the longest length of valid parentheses up to each index in the string. By iterating through each character and inspecting the state of previous indices, we build up a solution that allows efficient computation of valid substrings.

1. Create a `dp` array initialized to zero, with the length equal to the length of the string.
2. Iterate over the string from the second character:
   - If the current character is `)` and the preceding character is `(`, they form a simple valid pair. Update `dp[i]` to be `dp[i-2] + 2`.
   - If the current character is `)` and the preceding character is also `)`, check if the character at position `i-dp[i-1]-1` is `(`. If true, update `dp[i]` to account for encapsulating valid parentheses.

3. The result is the maximum value in the `dp` array.

### Code:

```javascript
function longestValidParentheses(s) {
    let maxLength = 0;
    let dp = Array(s.length).fill(0);

    for (let i = 1; i < s.length; i++) {
        if (s[i] === ')') {
            if (s[i - 1] === '(') {
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2; // Simple pair found
            } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] === '(') {
                // Find encapsulated valid string
                dp[i] = dp[i - 1] + ((i - dp[i - 1]) >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
            }
            maxLength = Math.max(maxLength, dp[i]); // Update max length
        }
    }

    return maxLength;
}
```

### Time Complexity:

- O(n), iterating through the string once and performing constant-time updates.

### Space Complexity:

- O(n), for the `dp` array used to store lengths of valid strings at each index.

---

## Two-Pass Counter Approach

### Intuition:

The two-pass counter approach simplifies checking by using counting mechanisms to tally open and closed parentheses. By running two passes (left to right and right to left), we effectively manage balancing without additional memory storage for indices.

1. Initialize counters for left and right parentheses to zero.
2. In the first pass (left to right), if parentheses are balanced (`left === right`), update the max length.
3. Reset the counters when there is more right than left, implying it cannot be part of a valid substring.
4. In the second pass (right to left), follow the same steps but in reverse, ensuring unmatched lefts are handled.

### Code:

```javascript
function longestValidParentheses(s) {
    let left = 0, right = 0, maxLength = 0;

    // Left to right pass
    for (let i = 0; i < s.length; i++) {
        if (s[i] === '(') left++; else right++;

        if (left === right) {
            maxLength = Math.max(maxLength, 2 * right); // Count matched pairs
        } else if (right > left) {
            left = right = 0; // Reset counters
        }
    }

    left = right = 0;

    // Right to left pass
    for (let i = s.length - 1; i >= 0; i--) {
        if (s[i] === ')') right++; else left++;

        if (left === right) {
            maxLength = Math.max(maxLength, 2 * left); // Count matched pairs
        } else if (left > right) {
            left = right = 0; // Reset counters
        }
    }

    return maxLength;
}
```

### Time Complexity:

- O(n), iterating over the string twice for two directional passes.

### Space Complexity:

- O(1), only using constant extra space for counters.

