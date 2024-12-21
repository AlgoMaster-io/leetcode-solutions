# [Leetcode 921: Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approaches
- [Approach 1: Stack Method](#approach-1-stack-method)
- [Approach 2: Balance Counting](#approach-2-balance-counting)

### Approach 1: Stack Method

**Intuition:**

Using a stack is a common approach to solve problems related to parentheses since it helps in tracking unmatched opening parentheses. We can traverse the string and use the stack to help us determine the number of unmatched parentheses on both sides. Whenever we encounter a closing parenthesis `')'`, we check if there is a corresponding opening parenthesis `'('` on the stack to match it. If there's no match, it indicates that this closing parenthesis is unmatched.

**Detailed Steps:**

1. Initialize an empty stack to keep track of opening parentheses.
2. Traverse through each character in the string:
   - If it is an opening parenthesis `'('`, push it onto the stack.
   - If it is a closing parenthesis `')'`, check if the stack is non-empty (meaning there is an unmatched `'('`). If non-empty, pop the top of the stack; otherwise, increment a counter to denote an unmatched `')'`.
3. After processing the entire string, the size of the stack will represent the number of unmatched `'('` and the counter will represent the unmatched `')'`.
4. The total number of parentheses to add will be the sum of unmatched `'('` and unmatched `')'` count.

**Time Complexity:** O(n), where n is the length of the string.
**Space Complexity:** O(n) due to the stack.

```javascript
function minAddToMakeValid(s) {
    let stack = [];
    let unmatchedClose = 0; // Counter for unmatched closing parenthesis
    
    for (let char of s) {
        if (char === '(') {
            // Push opening parentheses onto the stack
            stack.push(char);
        } else {
            // For closing parenthesis, check for a matching opening parenthesis
            if (stack.length > 0) {
                stack.pop(); // Match found, pop from the stack
            } else {
                unmatchedClose++; // No match found, increment unmatched counter
            }
        }
    }
    
    // Unmatched opening parentheses remain in the stack
    return unmatchedClose + stack.length;
}
```

### Approach 2: Balance Counting

**Intuition:**

This approach avoids using a stack and instead utilizes two variables to keep track of unmatched parentheses. We maintain a balance of unmatched opening parentheses. For any unmatched `')'`, if balance is zero, it means it adds to our unmatched count on the closing side. Otherwise, a closing parenthesis reduces the existing unmatched `'('` balance.

**Detailed Steps:**

1. Initialize two counters: `balance` for unmatched opening parentheses and `count` for the number of unmatched close parentheses needed.
2. Traverse through each character in the string:
   - If it's `'('`, increment the `balance`.
   - If it's `')'`:
     - If `balance` is greater than 0, it means there is a `'('` to match, so decrement `balance`.
     - Otherwise, increment `count` because this indicates an unmatched `')'`.
3. The result will be the sum of `balance` (unmatched `'('` left) and `count` (unmatched `')'`).

**Time Complexity:** O(n), where n is the length of the string.
**Space Complexity:** O(1) as it uses constant extra space.

```javascript
function minAddToMakeValid(s) {
    let balance = 0;
    let count = 0; // Counter for unmatched closing parenthesis

    for (let char of s) {
        if (char === '(') {
            balance++; // Increment balance for an opening parenthesis
        } else {
            if (balance > 0) {
                balance--; // Closing parenthesis matches an unmatched opening parenthesis
            } else {
                count++; // Increment count for unmatched closing parenthesis
            }
        }
    }
    
    // Add remaining balance (unmatched opening parentheses) to count
    return count + balance;
}
```

These are two effective approaches to solve the problem; `Approach 2` generally performs better due to reduced space usage and simpler control flow.

