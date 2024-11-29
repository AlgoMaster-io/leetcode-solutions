# [1047. Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)

## Approach: Using a Stack

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []

        # Iterate through the string
        for c in s:
            # If the stack is not empty and the top element matches the current character
            if stack and stack[-1] == c:
                stack.pop()  # Remove the top element
            else:
                stack.append(c)  # Add the current character to the stack

        return ''.join(stack)  # Convert stack back to a string
```

