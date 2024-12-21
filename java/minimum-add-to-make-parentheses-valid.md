# [Leetcode Problem 921: Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Table of Contents
- [Approach 1: Stack Based Solution](#approach-1-stack-based-solution)
- [Approach 2: Counting Unmatched Parentheses](#approach-2-counting-unmatched-parentheses)

### Approach 1: Stack Based Solution

**Intuition:**
The problem is to ensure that each open parenthesis '(' has a corresponding closing parenthesis ')'. A simple approach involves using a stack data structure to keep track of unmatched parentheses. We iterate through the string, pushing open parentheses onto the stack and popping when we encounter a closing parenthesis that matches. If a closing parenthesis has no matching open parenthesis (the stack is empty), it indicates an unmatched closing parenthesis. By the end of the string traversal, the size of the stack plus any unmatched closing parentheses encountered gives the minimum additions needed to make the string valid.

**Solution:**

```java
public int minAddToMakeValid(String s) {
    Stack<Character> stack = new Stack<>();
    int unmatchedClose = 0;

    for (char c : s.toCharArray()) {
        if (c == '(') {
            stack.push(c);
        } else { // c == ')'
            if (!stack.isEmpty()) {
                stack.pop();
            } else {
                unmatchedClose++;
            }
        }
    }

    // Stack contains unmatched open parentheses
    return stack.size() + unmatchedClose;
}
```

**Time Complexity:** O(n), where n is the length of the input string, since we pass through the string once.

**Space Complexity:** O(n), as we may store all the characters in the stack in the worst case.

### Approach 2: Counting Unmatched Parentheses

**Intuition:**
Instead of using a stack, we can optimize the space by simply counting the number of unmatched open and close parentheses. We maintain two counters: one for unmatched open parentheses (`open`) and another for unmatched closing parentheses (`close`). As we iterate through the string, for every `(` encountered, we increment the `open` counter. For every `)`, if there is a corresponding unmatched open parenthesis, we decrement the `open` counter (effectively "matching" them). Otherwise, we increment the `close` counter indicating an unmatched `)`.

**Solution:**

```java
public int minAddToMakeValid(String s) {
    int open = 0, close = 0;

    for (char c : s.toCharArray()) {
        if (c == '(') {
            open++;
        } else { // c == ')'
            if (open > 0) {
                open--; // Match the current ')' with a previous '('
            } else {
                close++; // No matching '(' for this ')'
            }
        }
    }

    // Total unmatched parentheses is sum of unmatched open and close
    return open + close;
}
```

**Time Complexity:** O(n), where n is the length of the input string, as we scan through the string once.

**Space Complexity:** O(1), since we only use a fixed amount of extra space for the `open` and `close` counters. 

By transitioning from the stack-based approach to counting unmatched parentheses, we optimize the space complexity from O(n) to O(1) whilst maintaining the same time complexity.

