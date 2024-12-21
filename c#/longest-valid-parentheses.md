# [Leetcode 32: Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach](#dynamic-programming-approach)
3. [Stack Approach](#stack-approach)
4. [Two-Pointer Approach](#two-pointer-approach)

---

### Brute Force Approach

The brute force method involves checking every possible substring to determine if it is a valid parentheses string. After identifying all the valid substrings, the longest one is selected.

**Intuition:**

- Generate all possible substrings of the given string.
- Check each substring for validity (equal number of '(' and ')', and every prefix should have more or equal '(' than ')').
- Track and update the maximum length of valid substrings.

**Time Complexity:** O(n^3)  
**Space Complexity:** O(n)

```csharp
public class Solution {
    public int LongestValidParentheses(string s) {
        int maxLength = 0;

        for (int i = 0; i < s.Length; i++) {
            for (int j = i + 2; j <= s.Length; j += 2) {
                if (IsValid(s.Substring(i, j - i))) {
                    maxLength = Math.Max(maxLength, j - i);
                }
            }
        }

        return maxLength;
    }

    private bool IsValid(string str) {
        int balance = 0;
        foreach (char c in str) {
            if (c == '(') {
                balance++;
            } else {
                balance--;
            }
            if (balance < 0) return false;
        }

        return balance == 0;
    }
}
```

### Dynamic Programming Approach

The dynamic programming approach uses an array to store the lengths of the longest valid parentheses substrings ending at each position.

**Intuition:**

- Create a DP array where `dp[i]` holds the length of the longest valid substring ending at index `i`.
- Iterate through the string. When a ')' is encountered, check if it can form a valid substring with a preceding '('.
- If it forms a valid substring, update the DP array accordingly.

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

```csharp
public class Solution {
    public int LongestValidParentheses(string s) {
        int maxLength = 0;
        int[] dp = new int[s.Length];

        for (int i = 1; i < s.Length; i++) {
            if (s[i] == ')') {
                // Check for immediate pair () case
                if (s[i - 1] == '(') {
                    dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
                } 
                // Check for nested pair case
                else if (i - dp[i - 1] > 0 && s[i - dp[i - 1] - 1] == '(') {
                    dp[i] = dp[i - 1] + ((i - dp[i - 1] >= 2) ? dp[i - dp[i - 1] - 2] : 0) + 2;
                }

                maxLength = Math.Max(maxLength, dp[i]);
            }
        }

        return maxLength;
    }
}
```

### Stack Approach

Using a stack, this method keeps track of indices of unmatched parentheses.

**Intuition:**

- Use a stack to store indices of unmatched '('.
- Initialize stack with -1 to handle edge cases of valid substring starting at the beginning of the string.
- Iterate through the string. For each ')', check for the matching '(' using the stack.

**Time Complexity:** O(n)  
**Space Complexity:** O(n)

```csharp
public class Solution {
    public int LongestValidParentheses(string s) {
        int maxLength = 0;
        Stack<int> stack = new Stack<int>();
        stack.Push(-1); // Base for the first valid substring

        for (int i = 0; i < s.Length; i++) {
            if (s[i] == '(') {
                stack.Push(i);
            } else {
                stack.Pop(); // Match current ')' with previous '('
                if (stack.Count == 0) {
                    stack.Push(i); // Record the position of unmatched ')'
                } else {
                    maxLength = Math.Max(maxLength, i - stack.Peek());
                }
            }
        }

        return maxLength;
    }
}
```

### Two-Pointer Approach

The two-pointer approach traverses the string twice - once from left to right and once from right to left.

**Intuition:**

- Traverse left to right, count '(' and ')' and check if counts match to form valid parentheses.
- Traverse right to left to handle cases of excess '(' without matching ')' at the end.

**Time Complexity:** O(n)  
**Space Complexity:** O(1)

```csharp
public class Solution {
    public int LongestValidParentheses(string s) {
        int left = 0, right = 0, maxLength = 0;

        // Left to Right
        for (int i = 0; i < s.Length; i++) {
            if (s[i] == '(') left++;
            else right++;

            if (left == right) {
                maxLength = Math.Max(maxLength, 2 * right);
            } else if (right > left) {
                left = right = 0;
            }
        }

        left = right = 0;
        // Right to Left
        for (int i = s.Length - 1; i >= 0; i--) {
            if (s[i] == '(') left++;
            else right++;

            if (left == right) {
                maxLength = Math.Max(maxLength, 2 * left);
            } else if (left > right) {
                left = right = 0;
            }
        }

        return maxLength;
    }
}
```

Each approach offers a unique perspective on tackling the problem, with the two-pointer and stack methods offering the most clean and efficient solutions.

