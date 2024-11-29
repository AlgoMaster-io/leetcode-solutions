# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if len(t) > len(s):
            return ""

        result = ""
        minLength = float('inf')

        # Iterate over all substrings
        for i in range(len(s)):
            for j in range(i, len(s)):
                sub = s[i:j + 1]

                if self.containsAll(sub, t):
                    if len(sub) < minLength:
                        minLength = len(sub)
                        result = sub

        return result

    def containsAll(self, sub: str, t: str) -> bool:
        t_count = Counter(t)

        for char in sub:
            if char in t_count:
                t_count[char] -= 1
                if t_count[char] == 0:
                    del t_count[char]

        return not t_count
```

## Approach 2: Sliding Window with Frequency Count (Optimal)

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
from collections import Counter, defaultdict

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if len(t) > len(s):
            return ""

        t_freq = Counter(t)
        window_freq = defaultdict(int)

        left, right = 0, 0
        matched = 0
        minLength = float('inf')
        start = 0

        while right < len(s):
            c = s[right]
            window_freq[c] += 1

            if c in t_freq and window_freq[c] == t_freq[c]:
                matched += 1

            while matched == len(t_freq):
                # Update the result if this window is smaller
                if right - left + 1 < minLength:
                    minLength = right - left + 1
                    start = left

                # Shrink the window
                left_char = s[left]
                if left_char in t_freq and window_freq[left_char] == t_freq[left_char]:
                    matched -= 1
                window_freq[left_char] -= 1
                if window_freq[left_char] == 0:
                    del window_freq[left_char]
                left += 1

            right += 1

        return "" if minLength == float('inf') else s[start:start + minLength]
```

