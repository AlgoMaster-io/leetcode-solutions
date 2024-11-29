# [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n^2)
# Space Complexity: O(1)
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        maxLength = 0

        # Iterate through every possible substring
        for i in range(len(s)):
            freq = [0] * 26
            maxFreq = 0

            for j in range(i, len(s)):
                freq[ord(s[j]) - ord('A')] += 1
                maxFreq = max(maxFreq, freq[ord(s[j]) - ord('A')])

                # Check if the current substring can be made uniform
                if (j - i + 1) - maxFreq <= k:
                    maxLength = max(maxLength, j - i + 1)

        return maxLength
```

## Approach 2: Sliding Window (Optimal)

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        freq = [0] * 26
        maxFreq = 0
        maxLength = 0
        left = 0

        # Sliding window
        for right in range(len(s)):
            freq[ord(s[right]) - ord('A')] += 1
            maxFreq = max(maxFreq, freq[ord(s[right]) - ord('A')])

            # If the remaining characters exceed k, shrink the window
            while (right - left + 1) - maxFreq > k:
                freq[ord(s[left]) - ord('A')] -= 1
                left += 1

            # Update maxLength
            maxLength = max(maxLength, right - left + 1)

        return maxLength
```

