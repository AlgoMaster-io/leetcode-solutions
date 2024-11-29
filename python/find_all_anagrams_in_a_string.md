# [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Approach 1: Brute Force (Basic)

### Solution
```python
# Time Complexity: O(n * m), where n is the length of s and m is the length of p
# Space Complexity: O(m) for frequency array

from typing import List

class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        result = []
        pFreq = [0] * 26
        
        # Frequency count for string p
        for c in p:
            pFreq[ord(c) - ord('a')] += 1
        
        # Check every substring of length p in s
        for i in range(len(s) - len(p) + 1):
            sFreq = [0] * 26
            
            # Count frequency of the current substring
            for j in range(i, i + len(p)):
                sFreq[ord(s[j]) - ord('a')] += 1
            
            # Compare frequencies
            if sFreq == pFreq:
                result.append(i)
        
        return result
```

## Approach 2: Sliding Window with Frequency Array (Optimal)

### Solution
```python
# Time Complexity: O(n), where n is the length of s
# Space Complexity: O(1) (fixed frequency array of size 26)

from typing import List

class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        result = []
        pFreq = [0] * 26
        sFreq = [0] * 26

        # Frequency count for string p
        for c in p:
            pFreq[ord(c) - ord('a')] += 1

        left = 0
        right = 0

        # Sliding window over s
        while right < len(s):
            # Add the current character to the window
            sFreq[ord(s[right]) - ord('a')] += 1

            # Check if the window size matches p
            if right - left + 1 == len(p):
                # Compare frequencies
                if sFreq == pFreq:
                    result.append(left)

                # Remove the leftmost character from the window
                sFreq[ord(s[left]) - ord('a')] -= 1
                left += 1

            right += 1

        return result
```

