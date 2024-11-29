# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
```python
# Time Complexity: O(n * m)
# Space Complexity: O(n)
# where n is the number of digits in N and m is the number of digits in D
class Solution:
    def atMostNGivenDigitSet(self, digits, n):
        n_str = str(n)
        len_n = len(n_str)
        digit_arr = [d for d in digits]
        
        def backtrack(digits, n_str, index, limit):
            if index == len(n_str):
                return 1
            
            count = 0
            for d in digits:
                if limit and d > n_str[index]:
                    break
                count += backtrack(digits, n_str, index + 1, limit and d == n_str[index])
            
            if not limit:
                factor = 1
                for i in range(index + 1, len(n_str)):
                    factor *= len(digits)
                count += factor
            
            return count
        
        return backtrack(digit_arr, n_str, 0, True)
```

## Approach 2: Dynamic Programming

### Solution
```python
# Time Complexity: O(n * m)
# Space Complexity: O(n)
# where n is the number of digits in N and m is the number of digits in D
class Solution:
    def atMostNGivenDigitSet(self, digits, n):
        n_str = str(n)
        len_n = len(n_str)
        dp = [0] * (len_n + 1)
        dp[len_n] = 1

        digit_arr = [d for d in digits]

        for i in range(len_n - 1, -1, -1):
            c = n_str[i]
            for d in digit_arr:
                if d < c:
                    dp[i] += len(digits) ** (len_n - i - 1)
                elif d == c:
                    dp[i] += dp[i + 1]

        result = 0
        for i in range(1, len_n):
            result += len(digits) ** i
        return result + dp[0]
```

## Approach 3: Mathematical Counting

### Solution
```python
# Time Complexity: O(n * m)
# Space Complexity: O(1)
# where n is the number of digits in N and m is the number of digits in D
class Solution:
    def atMostNGivenDigitSet(self, digits, n):
        n_str = str(n)
        len_n = len(n_str)
        count = [[0, 0] for _ in range(len_n)]

        # Precompute the power array to avoid repetitive calculation
        power = [1] * (len_n + 1)
        for i in range(1, len_n + 1):
            power[i] = power[i - 1] * len(digits)

        for i in range(len_n):
            c = n_str[i]
            for digit in digits:
                d = digit[0]
                if d < c:
                    count[i][0] += power[len_n - i - 1]
                elif d == c:
                    count[i][1] = 1
            if count[i][1] == 0:
                break

        result = sum(c[0] for c in count)

        for i in range(1, len_n):
            result += power[i]

        result += count[len_n - 1][1]

        return result
```

