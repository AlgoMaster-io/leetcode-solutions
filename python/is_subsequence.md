# 392. [Is Subsequence](https://leetcode.com/problems/is-subsequence/)

## Approach 1: Two Pointers (Greedy)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        s_index = 0  # Pointer for string `s`
        t_index = 0  # Pointer for string `t`

        # Traverse both strings
        while s_index < len(s) and t_index < len(t):
            if s[s_index] == t[t_index]:
                s_index += 1  # Move pointer in `s` if characters match
            t_index += 1  # Always move pointer in `t`

        # If s_index has reached the end of `s`, it's a subsequence
        return s_index == len(s)
```

## Approach 2: Iterative with Character Search

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        index = -1  # Stores the last found position of characters

        # Iterate through each character in `s`
        for c in s:
            # Find the next occurrence of `c` in `t` after the previous index
            index = t.find(c, index + 1)
            if index == -1:
                # If character not found, `s` is not a subsequence of `t`
                return False

        # All characters of `s` found in `t` in order
        return True
```

## Approach 3: Binary Search (For Multiple Queries)

Use this approach when checking multiple subsequences against the same string t.

### Solution
```python
# Time Complexity: O(n + m log k) for preprocessing + query
# Space Complexity: O(n)
import bisect
from collections import defaultdict

class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        # Preprocess `t`: Create a map of character positions
        char_positions = defaultdict(list)
        for i, char in enumerate(t):
            char_positions[char].append(i)

        last_position = -1  # Tracks the position of the last matched character

        # Check each character in `s`
        for c in s:
            if c not in char_positions:
                return False  # If `c` not in `t`, return false

            # Perform binary search to find the next valid position
            positions = char_positions[c]
            next_position = bisect.bisect_right(positions, last_position)
            
            if next_position == len(positions):
                return False  # No valid position found

            last_position = positions[next_position]  # Update last_position

        return True
```

