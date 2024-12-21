# [Leetcode 921: Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approaches:
- [Approach 1: Stack](#approach-1-stack)
- [Approach 2: Two Counters (Optimal)](#approach-2-two-counters-optimal)

---

## Approach 1: Stack

### Intuition:
To determine the minimum number of parenthesis additions required to make a string valid, one can use a stack to track unmatched parentheses. The basic idea is to push opening parentheses `'('` onto the stack and pop them when a closing parenthesis `')'` is encountered. Any unmatched opening or closing parentheses indicate a need for additional parentheses to balance the expression.

### Steps:
1. Initialize an empty stack.
2. Traverse through each character in the input string:
   - If the character is `'('`, push it onto the stack.
   - If the character is `')'` and the stack is not empty, pop an element from the stack.
   - If the character is `')'` and the stack is empty, this means there is no matching `'('`, so we increment a counter to keep track of unmatched `')'`.
3. The number of unmatched `'('` will be equal to the size of the stack after traversal.
4. The sum of unmatched `')'` and the remaining size of the stack will give the total number of additions required.

### Code:
```csharp
public int MinAddToMakeValid(string s) {
    Stack<char> stack = new Stack<char>();
    int unmatchedClose = 0;
    
    foreach (char c in s) {
        if (c == '(') {
            // Push '(' onto the stack
            stack.Push(c);
        } else {
            // Check if there's a matching '(' in the stack for ')'
            if (stack.Count > 0) {
                stack.Pop();
            } else {
                // No matching '(', increment unmatchedClose
                unmatchedClose++;
            }
        }
    }
    
    // Total additions needed = unmatched '(' + unmatched ')'
    return stack.Count + unmatchedClose;
}
```

### Time Complexity:
- **O(n)** where n is the length of the string. We traverse the string once.

### Space Complexity:
- **O(n)** in the worst case if all characters are `'('` and are pushed to the stack.

---

## Approach 2: Two Counters (Optimal)

### Intuition:
We can optimize the stack approach by eliminating the need for extra space used by the stack. We only need two counters, one for open unmatched parentheses and another for unmatched closing parentheses.

### Steps:
1. Initialize two counters: `open` for unmatched `'('` and `close` for unmatched `')'`.
2. Traverse through the string:
   - If the character is `'('`, increment the `open` counter.
   - If the character is `')'`:
     - If `open` is greater than zero, this means there is a potential for the `')'` to match with a `'('`, so decrement `open`.
     - Otherwise, increment `close` indicating an unmatched `')'`.
3. The total number of additions required will be the sum of `open` and `close`.

### Code:
```csharp
public int MinAddToMakeValid(string s) {
    int open = 0, close = 0;
    
    foreach (char c in s) {
        if (c == '(') {
            // Increment open count
            open++;
        } else {
            // Try to match ')' with '('
            if (open > 0) {
                open--; // Match one '('
            } else {
                close++; // No matching '(', increase count of unmatched ')'
            }
        }
    }
    
    // Total additions needed = unmatched '(' + unmatched ')'
    return open + close;
}
```

### Time Complexity:
- **O(n)** where n is the length of the string. We go through the string once.

### Space Complexity:
- **O(1)** as we are using only constant space for two counters.

