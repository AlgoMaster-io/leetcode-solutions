# [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)

## Approach: Monotonic Stack with Character Frequency Tracking

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1) (fixed-size stack and frequency array)
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        last_occurrence = [0] * 26  # Store last index of each character
        in_stack = [False] * 26     # Track if character is already in the stack

        # Calculate last occurrence of each character
        for i, char in enumerate(s):
            last_occurrence[ord(char) - ord('a')] = i

        stack = []

        # Iterate through the string
        for i, char in enumerate(s):
            if in_stack[ord(char) - ord('a')]:
                continue  # Skip if the character is already in the stack

            # Remove characters from the stack that are greater than the current character
            # and can appear later in the string
            while stack and stack[-1] > char and last_occurrence[ord(stack[-1]) - ord('a')] > i:
                removed_char = stack.pop()
                in_stack[ord(removed_char) - ord('a')] = False

            # Push the current character onto the stack
            stack.append(char)
            in_stack[ord(char) - ord('a')] = True

        # Build the result string from the stack
        return ''.join(stack)
```

