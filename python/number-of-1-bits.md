# [Leetcode Problem 191: Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approaches
- [Approach 1: Loop and Bit Shift](#approach-1-loop-and-bit-shift)
- [Approach 2: Built-in Python Function](#approach-2-built-in-python-function)
- [Approach 3: Brian Kernighan’s Algorithm](#approach-3-brian-kernighans-algorithm)

---

## Approach 1: Loop and Bit Shift

### Intuition
To count the number of 1 bits in an integer, we can repeatedly check the least significant bit (LSB) and then shift the bits of the number to the right. The bitwise AND operation with `1` helps to isolate the LSB: `n & 1` will be `1` if the LSB is `1` and `0` otherwise. By right shifting the number by 1 at each step and checking the LSB, we can count all the 1 bits.

### Time Complexity
- **Time Complexity**: O(32) = O(1), since we are guaranteed a 32-bit integer.
- **Space Complexity**: O(1), as we only use a constant amount of space.

### Code

```python
def hammingWeight(n: int) -> int:
    count = 0
    while n:
        # If the last bit is 1, increment the count
        count += n & 1
        # Shift bits of n right by 1
        n >>= 1
    return count
```

---

## Approach 2: Built-in Python Function

### Intuition
Python provides built-in capabilities to convert numbers to their binary representation using `bin()`. We can leverage this by converting the number to a binary string and then simply counting the number of '1's.

### Time Complexity
- **Time Complexity**: O(1), the operation involves converting a 32-bit number and counting, which is constant time.
- **Space Complexity**: O(1), the space usage is constant, only variable space usage for the conversion.

### Code

```python
def hammingWeight(n: int) -> int:
    # Convert integer to binary string and count '1's
    return bin(n).count('1')
```

---

## Approach 3: Brian Kernighan’s Algorithm

### Intuition
Brian Kernighan's Algorithm is more efficient as it reduces the number of iterations in counting the number of 1 bits. The core idea is to repeatedly turn off the rightmost bit that is `1`. This is done by performing `n = n & (n - 1)` which removes the lowest set bit.

### Time Complexity
- **Time Complexity**: O(k), where `k` is the number of 1 bits; in the worst case, it is O(32) = O(1) for a 32-bit integer.
- **Space Complexity**: O(1), constant space usage.

### Code

```python
def hammingWeight(n: int) -> int:
    count = 0
    while n:
        # Turn off the rightmost 1-bit
        n &= n - 1
        # Increment count as a 1-bit is removed
        count += 1
    return count
```

Each method provides a valid way to solve the problem, with Brian Kernighan's Algorithm offering an efficient on-the-fly reduction in the number of operations needed by directly manipulating the bits.

