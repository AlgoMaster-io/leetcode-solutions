# [202. Happy Number](https://leetcode.com/problems/happy-number/)

## Approach 1: Using HashSet to Detect Cycles

### Solution
python
```python
# Time Complexity: O(log(n))
# Space Complexity: O(log(n))
class Solution:
    def isHappy(self, n: int) -> bool:
        seen = set()

        while n != 1 and n not in seen:
            seen.add(n)
            n = self.getNextNumber(n)

        return n == 1

    def getNextNumber(self, n: int) -> int:
        sum = 0

        while n > 0:
            digit = n % 10
            sum += digit * digit
            n //= 10

        return sum
```

## Approach 2: Fast and Slow Pointers (Optimal for Cycle Detection)

### Solution
python
```python
# Time Complexity: O(log(n))
# Space Complexity: O(1)
class Solution:
    def isHappy(self, n: int) -> bool:
        slow = n
        fast = self.getNextNumber(n)

        while fast != 1 and slow != fast:
            slow = self.getNextNumber(slow)
            fast = self.getNextNumber(self.getNextNumber(fast))

        return fast == 1

    def getNextNumber(self, n: int) -> int:
        sum = 0

        while n > 0:
            digit = n % 10
            sum += digit * digit
            n //= 10

        return sum
```

