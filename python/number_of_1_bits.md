# 191. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approach 1: Loop and Bit Manipulation

### Solution
python
```python
# Time Complexity: O(32) because an integer in Python is simulated as infinite precision.
# Space Complexity: O(1)
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n:
            # Check if the least significant bit is 1
            count += n & 1
            # Right shift to process the next bit
            n >>= 1  # Python does not have an unsigned shift, but this works for positive integers
        return count
```

## Approach 2: Brian Kernighan’s Algorithm

### Solution
python
```python
# Time Complexity: O(1) in the sense that the operation is related to the number of 1s.
# Space Complexity: O(1)
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        while n:
            # Drop the lowest set bit
            n &= n - 1
            # Increment count for each bit cleared
            count += 1
        return count
```

## Approach 3: Using Built-in Function

### Solution
python
```python
# Time Complexity: O(1) as the function call is executing in a constant time.
# Space Complexity: O(1)
class Solution:
    def hammingWeight(self, n: int) -> int:
        # Use Python's bin() method and count the number of 1s
        return bin(n).count('1')
```

