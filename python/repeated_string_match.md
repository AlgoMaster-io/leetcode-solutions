# 686. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)

## Approach 1: Brute Force

### Solution
```python
# Time Complexity: O(n * m), where n is the length of A and m is the length of B
# Space Complexity: O(n + m)
class Solution:
    def repeatedStringMatch(self, A: str, B: str) -> int:
        repeatedA = A
        repeatCount = 1

        # Repeat A until it's length is at least length of B
        while len(repeatedA) < len(B):
            repeatedA += A
            repeatCount += 1

        # Check if B is a substring of the repeated A
        if B in repeatedA:
            return repeatCount

        # Add one more repetition just in case B starts towards the end
        repeatedA += A
        if B in repeatedA:
            return repeatCount + 1

        return -1  # B is not a substring of repeated A
```

## Approach 2: Optimized Approach using Rolling Window

### Solution
```python
# Time Complexity: O(n + m), where n is the length of A and m is the length of B
# Space Complexity: O(n + m)
class Solution:
    def repeatedStringMatch(self, A: str, B: str) -> int:
        m = len(A)
        n = len(B)
        maxRepeat = (n // m) + 2  # Only need to repeat at most this many times

        repeatedA = ""
        for i in range(maxRepeat):
            repeatedA += A
            # Check if B is a substring of the current repeated A
            if B in repeatedA:
                return i + 1

        return -1  # B is not a substring of repeated A
```

