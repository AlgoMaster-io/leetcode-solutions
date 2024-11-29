# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## Approach: Using a Stack

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)

class Solution:
    def isValid(self, s: str) -> bool:
        stack = []

        # Helper function to check matching pairs
        def is_matching_pair(opening, closing):
            return (opening == '(' and closing == ')') or \
                   (opening == '{' and closing == '}') or \
                   (opening == '[' and closing == ']')
        
        # Iterate through the string
        for char in s:
            # Push opening brackets onto the stack
            if char in "({[":
                stack.append(char)
            # Check closing brackets
            else:
                # If stack is empty or doesn't match the top, return false
                if not stack or not is_matching_pair(stack.pop(), char):
                    return False

        # If the stack is empty, all brackets are matched
        return len(stack) == 0
```

