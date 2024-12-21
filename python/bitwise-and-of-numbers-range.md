# [Leetcode Problem 201: Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches:
- [Naive Iteration](#naive-iteration)
- [Bit Manipulation Reduction](#bit-manipulation-reduction)
- [Shift Right](#shift-right)

### Naive Iteration

**Intuition**:  
The naive approach would be to iterate over every number in the range `[m, n]` and compute the bitwise AND. While not efficient, it gives a clear understanding of what needs to be achieved. This just tries to AND all the numbers from m to n.

```python
def rangeBitwiseAnd(m: int, n: int) -> int:
    # Start with the initial number
    result = m
    # Iterate through the numbers from m to n
    for num in range(m+1, n+1):
        result &= num
        # Early stop if result becomes zero, because AND with 0 will always be zero
        if result == 0:
            break
    return result
```

**Time Complexity**: O(n - m), where `n` is the upper range limit  
**Space Complexity**: O(1)

### Bit Manipulation Reduction

**Intuition**:  
When bitwise ANDing a large range of numbers, consider the common bits that are set across all numbers. As soon as bits start differing, the result will start dropping to zero from those positions onwards.

```python
def rangeBitwiseAnd(m: int, n: int) -> int:
    # Reduce the range by repeatedly zeroing out the least significant bit of n 
    # until n is less than or equal to m.
    while m < n:
        # Remove the least significant 1-bit from n
        n = n & (n - 1)
    return n
```

**Time Complexity**: O(log n), since we are progressively zeroing out significant bits  
**Space Complexity**: O(1)

### Shift Right

**Intuition**:  
The core idea here is that bitwise AND between `m` and `n` will result in numbers where only the leftmost common bits remain. By shifting both `m` and `n` right until they are equal, we effectively remove the varying tail bits.

```python
def rangeBitwiseAnd(m: int, n: int) -> int:
    # Initialize a counter for shifted bits
    shift = 0
    # Shift both numbers right until they are the same
    while m < n:
        m >>= 1
        n >>= 1
        shift += 1
    # Shift back to the original positions
    return m << shift
```

**Time Complexity**: O(log n), as we are shifting bits right until `m` and `n` are aligned  
**Space Complexity**: O(1)

Each of these solutions approaches the problem from different angles, from brute force iteration to more sophisticated bit manipulations. The "Shift Right" method is particularly efficient for large ranges. It captures the essence that bits remaining after several right shifts are inherently in all range numbers.

