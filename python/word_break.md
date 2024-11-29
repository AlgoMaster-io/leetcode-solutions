# 139. [Word Break](https://leetcode.com/problems/word-break/)

## Approach 1: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
from typing import List

class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        return self.wordBreakHelper(s, wordDict, {})

    def wordBreakHelper(self, s: str, wordDict: List[str], memo: dict) -> bool:
        if not s:
            return True
        
        if s in memo:
            return memo[s]

        # Check if any prefix of 's' is a word in wordDict
        for word in wordDict:
            if s.startswith(word) and self.wordBreakHelper(s[len(word):], wordDict, memo):
                memo[s] = True
                return True

        memo[s] = False
        return False
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n^2)
# Space Complexity: O(n)
from typing import List

class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        wordSet = set(wordDict)
        dp = [False] * (len(s) + 1)
        dp[0] = True  # Base case, empty string

        # Traverse the string to build DP table
        for i in range(1, len(s) + 1):
            for j in range(i):
                # If substring(0, j) can be segmented and substring(j, i) is in wordSet
                if dp[j] and s[j:i] in wordSet:
                    dp[i] = True
                    break  # Move on to next i since we found a valid segmentation

        return dp[len(s)]  # Result is whether the whole string can be segmented
```

