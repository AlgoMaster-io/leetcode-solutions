# 91. [Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approach 1: Recursion with Memoization

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def numDecodings(self, s: str) -> int:
        return self.decode_helper(s, 0, {})

    def decode_helper(self, s: str, index: int, memo: dict) -> int:
        # If we've reached the end of the string, return 1 for a valid path
        if index == len(s):
            return 1

        # If the current portion of the string starts with '0', it's invalid
        if s[index] == '0':
            return 0

        # If we've already computed the result for this index, return it
        if index in memo:
            return memo[index]

        # Decode one character
        result = self.decode_helper(s, index + 1, memo)

        # Decode two characters if it's a valid two-digit number
        if index + 1 < len(s) and (s[index] == '1' or (s[index] == '2' and s[index + 1] < '7')):
            result += self.decode_helper(s, index + 2, memo)

        # Save the result to the memoization map
        memo[index] = result

        return result
```

## Approach 2: Dynamic Programming

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(n)
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == '0':
            return 0

        n = len(s)
        dp = [0] * (n + 1)

        dp[0] = 1  # Base case: an empty string has one way to decode
        dp[1] = 1  # Base case: the first character can't be '0'

        for i in range(2, n + 1):
            one_digit = int(s[i - 1:i])
            two_digits = int(s[i - 2:i])

            if 1 <= one_digit <= 9:
                dp[i] += dp[i - 1]

            if 10 <= two_digits <= 26:
                dp[i] += dp[i - 2]

        return dp[n]
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
python
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def numDecodings(self, s: str) -> int:
        if not s or s[0] == '0':
            return 0

        n = len(s)
        prev2 = 1  # dp[i-2]
        prev1 = 1  # dp[i-1]

        for i in range(1, n):
            current = 0
            one_digit = int(s[i:i + 1])
            two_digits = int(s[i - 1:i + 1])

            if 1 <= one_digit <= 9:
                current += prev1

            if 10 <= two_digits <= 26:
                current += prev2

            prev2 = prev1
            prev1 = current

        return prev1
```

