# [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)

## Approach 1: Brute Force (Basic)

### Solution
python
```python
# Time Complexity: O(n * m), where n is the length of s1 and m is the length of s2
# Space Complexity: O(n)
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        from collections import Counter
        
        n, m = len(s1), len(s2)
        
        if n > m:
            return False
        
        # Sort s1
        sorted_s1 = sorted(s1)

        # Check every substring of length n in s2
        for i in range(m - n + 1):
            if sorted_s1 == sorted(s2[i:i + n]):
                return True

        return False
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
python
```python
# Time Complexity: O(m), where m is the length of s2
# Space Complexity: O(1)
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        if len(s1) > len(s2):
            return False

        s1_freq = [0] * 26
        s2_freq = [0] * 26

        # Frequency count for s1
        for c in s1:
            s1_freq[ord(c) - ord('a')] += 1

        # Sliding window
        for i in range(len(s2)):
            s2_freq[ord(s2[i]) - ord('a')] += 1

            # Remove the leftmost character from the window
            if i >= len(s1):
                s2_freq[ord(s2[i - len(s1)]) - ord('a')] -= 1

            # Compare the frequency arrays
            if s1_freq == s2_freq:
                return True

        return False
```



