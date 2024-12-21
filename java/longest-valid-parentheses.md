# [Leetcode Problem 32: Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

## Navigation
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Using Stack](#approach-2-using-stack)
- [Approach 3: Optimized Two-Pass Counter](#approach-3-optimized-two-pass-counter)

## Approach 1: Brute Force

### Intuition
The simplest way to solve this problem is to generate all possible substrings and check each one to determine if it is a valid parentheses string. A valid parentheses string must have balanced opening and closing parentheses.

### Methodology
- Iterate over all possible substrings of the given string.
- For each substring, check if it is valid by ensuring that the count of '(' matches the count of ')'.
- Track the length of the longest valid substring found.

### Code
```java
public class Solution {
    public int longestValidParentheses(String s) {
        int maxLength = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 2; j <= s.length(); j += 2) { // Substrings of even length
                if (isValid(s.substring(i, j))) {
                    maxLength = Math.max(maxLength, j - i);
                }
            }
        }
        return maxLength;
    }

    private boolean isValid(String s) {
        int balance = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') {
                balance++;
            } else {
                balance--;
            }
            if (balance < 0) return false; // More ')' than '('
        }
        return balance == 0; // All '(' have a matching ')'
    }
}
```

### Complexity
- **Time Complexity:** \(O(n^3)\), due to generating and checking each substring.
- **Space Complexity:** \(O(n)\), to store substrings temporarily.

---

## Approach 2: Using Stack

### Intuition
A stack can help keep track of indices of unmatched '(' characters. Whenever a valid pair is found, calculate the length of valid substrings till then using these indices from the stack.

### Methodology
- Use the stack to store the indices of '(' characters.
- When a ')' is encountered, pop from the stack; if the stack is empty, current valid substring length is zero, otherwise calculate the length from the new top of the stack.
- Maintain a max length of such valid substrings.

### Code
```java
import java.util.Stack;

public class Solution {
    public int longestValidParentheses(String s) {
        Stack<Integer> stack = new Stack<>();
        stack.push(-1);  // Base for valid substring calculations
        int maxLength = 0;

        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.isEmpty()) {
                    stack.push(i);  // Mark the last unmatched ')'
                } else {
                    maxLength = Math.max(maxLength, i - stack.peek());
                }
            }
        }
        return maxLength;
    }
}
```

### Complexity
- **Time Complexity:** \(O(n)\), scan through the string once.
- **Space Complexity:** \(O(n)\), space used by the stack.

---

## Approach 3: Optimized Two-Pass Counter

### Intuition
Instead of using a stack, two traversals maintaining counters (`left` and `right`) to track opening '(' and closing ')' parentheses can be more space-efficient.

### Methodology
- Traverse the string from left to right:
  - Increment `left` counter for '(' and `right` counter for ')'.
  - If both counters are equal, update the max length found.
  - Reset counters if `right` becomes greater than `left`.
  
- Traverse again from right to left:
  - Use the same counters, but increment `right` for ')' and `left` for '(' to ensure all cases are checked.
  - Reset counters if `left` becomes greater than `right`.

### Code
```java
public class Solution {
    public int longestValidParentheses(String s) {
        int left = 0, right = 0, maxLength = 0;

        // Left to right traversal
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') left++;
            else right++;

            if (left == right) {
                maxLength = Math.max(maxLength, 2 * right);
            } else if (right > left) {
                left = right = 0; // Reset counters
            }
        }

        left = right = 0;

        // Right to left traversal
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) == '(') left++;
            else right++;

            if (left == right) {
                maxLength = Math.max(maxLength, 2 * left);
            } else if (left > right) {
                left = right = 0; // Reset counters
            }
        }

        return maxLength;
    }
}
```

### Complexity
- **Time Complexity:** \(O(n)\) for scanning the string twice.
- **Space Complexity:** \(O(1)\), constant space usage.

