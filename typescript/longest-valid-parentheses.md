# [Leetcode 32: Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Using Stack](#using-stack)
3. [Optimized with Dynamic Programming](#optimized-with-dynamic-programming)
4. [Two Pointers](#two-pointers)

---

### Brute Force Approach

We can start by checking every possible substring to determine if it is valid, and then return the length of the longest valid substring.

#### Intuition
- For each possible starting point of a substring, explore all the substrings starting at those indices.
- Validate each substring by checking if there are any unmatched parentheses.

#### TypeScript Implementation

```typescript
function isValid(s: string): boolean {
    let balance = 0;
    for (const char of s) {
        if (char === '(') {
            balance++;
        } else {
            balance--;
        }
        if (balance < 0) {
            return false;
        }
    }
    return balance === 0;
}

function longestValidParentheses(s: string): number {
    let maxLength = 0;
    for (let i = 0; i < s.length; i++) {
        for (let j = i + 2; j <= s.length; j += 2) {
            if (isValid(s.substring(i, j))) {
                maxLength = Math.max(maxLength, j - i);
            }
        }
    }
    return maxLength;
}
```

#### Complexity
- **Time Complexity:** O(n^3) - Two loops for creating substrings, and one for validation.
- **Space Complexity:** O(1) - No additional space required other than variables.

---

### Using Stack

Utilize a stack to keep track of indices of unmatched '(' characters.

#### Intuition
- Push each '(' index onto the stack.
- If encountering ')', pop from the stack.
  - If the stack is empty, push the current index as a new potential start of a valid substring.
  - If the stack is not empty, calculate the length of the current valid substring using the current index and the top index of the stack.

#### TypeScript Implementation

```typescript
function longestValidParenthesesUsingStack(s: string): number {
    let maxLength = 0;
    const stack: number[] = [-1]; // Add initial -1, to handle edge case for the start of a valid substring

    for (let i = 0; i < s.length; i++) {
        if (s[i] === '(') {
            stack.push(i);
        } else {
            stack.pop();
            if (stack.length === 0) {
                stack.push(i);
            } else {
                maxLength = Math.max(maxLength, i - stack[stack.length - 1]);
            }
        }
    }

    return maxLength;
}
```

#### Complexity
- **Time Complexity:** O(n) - Single pass through the string.
- **Space Complexity:** O(n) - Stack can potentially store all indices.

---

### Optimized with Dynamic Programming

Use dynamic programming to store results of subproblems (lengths of longest valid parentheses substrings ending at each index).

#### Intuition
- Use an array `dp` where `dp[i]` represents the length of the longest valid substring ending at index `i`.
- Iterate through the string:
  - When you encounter ')', if there is a matching '(', use previous `dp` values to update `dp[i]`.

#### TypeScript Implementation

```typescript
function longestValidParenthesesDP(s: string): number {
    let maxLength = 0;
    const dp = new Array(s.length).fill(0);

    for (let i = 1; i < s.length; i++) {
        if (s[i] === ')') {
            if (s[i - 1] === '(') {
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
            } else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] === '(') {
                dp[i] = dp[i - 1] + (i - dp[i - 1] >= 2 ? dp[i - dp[i - 1] - 2] : 0) + 2;
            }
            maxLength = Math.max(maxLength, dp[i]);
        }
    }

    return maxLength;
}
```

#### Complexity
- **Time Complexity:** O(n) - Single pass through the string.
- **Space Complexity:** O(n) - DP array.

---

### Two Pointers

Traverse the string while counting open and close parentheses using two passes; one from left to right, and another from right to left.

#### Intuition
- Left-to-right: Increment counters for '(' and ')'. When count of ')' matches '(', calculate current length as valid. Reset whenever ')' exceeds '('.
- Right-to-left: Repeat process, but reverse roles of '(' and ')'.

#### TypeScript Implementation

```typescript
function longestValidParenthesesTwoPointers(s: string): number {
    let maxLength = 0;
    let open = 0, close = 0;

    // Left to right
    for (const char of s) {
        if (char === '(') {
            open++;
        } else {
            close++;
        }
        if (open === close) {
            maxLength = Math.max(maxLength, 2 * close);
        } else if (close > open) {
            open = close = 0;
        }
    }

    open = close = 0;

    // Right to left
    for (let i = s.length - 1; i >= 0; i--) {
        if (s[i] === ')') {
            close++;
        } else {
            open++;
        }
        if (open === close) {
            maxLength = Math.max(maxLength, 2 * open);
        } else if (open > close) {
            open = close = 0;
        }
    }

    return maxLength;
}
```

#### Complexity
- **Time Complexity:** O(n) - Each pass processes the string.
- **Space Complexity:** O(1) - Only a few variables used.

This concludes the solutions to the problem "Longest Valid Parentheses" starting from the most basic to the most optimal solution with comprehensive explanations and TypeScript implementations.

