# 357. [Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approach 1: Recursive Backtracking

### Solution
```python
# Time Complexity: O(10^n)
# Space Complexity: O(n)
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n == 0:
            return 1
        
        def countNumbers(current: int, n: int, used: list[bool]) -> int:
            if n == 0:
                return 1
            count = 1  # count the number itself
            for i in range(10):
                if not used[i]:
                    used[i] = True
                    count += countNumbers(current * 10 + i, n - 1, used)
                    used[i] = False
            return count
        
        used = [False] * 10
        return countNumbers(0, n, used)
```

## Approach 2: Dynamic Programming

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n == 0:
            return 1

        uniqueNumbers = 10
        availableDigits = 9
        currentUnique = 9

        # Use dynamic programming to calculate the number
        for i in range(2, n + 1):
            currentUnique *= availableDigits
            uniqueNumbers += currentUnique
            availableDigits -= 1

        return uniqueNumbers
```

## Approach 3: Formula-based Calculation

### Solution
```python
# Time Complexity: O(n)
# Space Complexity: O(1)
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n == 0:
            return 1

        count = 10  # start counting from single-digit numbers
        uniqueDigits = 9
        availableNumbers = 9

        # Calculate using the permutation formula for unique digits
        while n > 1 and availableNumbers > 0:
            uniqueDigits *= availableNumbers
            count += uniqueDigits
            availableNumbers -= 1
            n -= 1

        return count
```

