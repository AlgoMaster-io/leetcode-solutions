# 921. [Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

## Approach: Counting Mismatches

### Solution
```python
# Time Complexity: O(n), where n is the length of the string
# Space Complexity: O(1)
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        open_count = 0  # Tracks unmatched '('
        close_count = 0  # Tracks unmatched ')'

        for c in s:
            if c == '(':
                open_count += 1  # Increment unmatched '('
            elif c == ')':
                if open_count > 0:
                    open_count -= 1  # Match ')' with a previous '('
                else:
                    close_count += 1  # Increment unmatched ')'

        # Total unmatched parentheses is the sum of unmatched '(' and ')'
        return open_count + close_count
```

