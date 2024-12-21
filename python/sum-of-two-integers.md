### [Leetcode Problem 371: Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

## Approaches:
- [Approach 1: Using Bit Manipulation with a Basic Loop](#approach-1-using-bit-manipulation-with-a-basic-loop)
- [Approach 2: Using Bit Manipulation with Python Built-ins](#approach-2-using-bit-manipulation-with-python-built-ins)

---

### Approach 1: Using Bit Manipulation with a Basic Loop

**Intuition:**

The problem requires us to sum two integers without using the `+` or `-` operators. The addition operation can be decomposed into a series of bit manipulations:

1. XOR (`^`): This operation will add two bits as if it were an addition without carrying (i.e., 0+0=0, 1+0=1, 0+1=1, 1+1=0).
2. AND (`&`): This will identify the bits that result in a carry when adding the two numbers.
3. Left shift (`<<`): This is used to shift the carry by one bit to align it with the original number.

We repeat this process iteratively until there are no more carries.

**Python Code:**

```python
def getSum(a: int, b: int) -> int:
    # 32 bits integer max
    MAX = 0x7FFFFFFF
    # Mask for bits beyond 32
    MASK = 0xFFFFFFFF
    # Iterating until there are no carries
    while b != 0:
        # Carry is AND, shifted left by 1
        carry = (a & b) << 1
        # Add without carry using XOR
        a = (a ^ b) & MASK
        # Apply mask to carry
        b = carry & MASK
    # If a is negative, use ~ operation with MASK
    return a if a <= MAX else ~(a ^ MASK)
```

**Time Complexity:** O(1) - The process runs in constant time because no matter what the input is, the loop will execute a finite number of times (bounded by the number of bits in an integer).

**Space Complexity:** O(1) - Only a limited number of integer variables are used.

---

### Approach 2: Using Bit Manipulation with Python Built-ins

**Intuition:**

This approach leverages Python's built-in support for arbitrarily large integers alongside the bit manipulation logic. The basic concept remains the same as in the previous approach. Python's dynamic typing and management of integer overflow aid in handling edge cases. We simply loop until there are no more carries, and Python takes care of number representation.

**Python Code:**

```python
def getSum(a: int, b: int) -> int:
    # Use mask to simulate 32-bit integer overflow
    MASK = 0xFFFFFFFF
    # Apply twos complement for negative numbers
    while b != 0:
        carry = (a & b) << 1  # Carry computation
        a = (a ^ b) & MASK    # Sum without carries
        b = carry & MASK      # Carry bits
    # Result checks, accounting for Python's handling of broader int types
    return a if a <= 0x7FFFFFFF else ~(a ^ MASK)
```

**Time Complexity:** O(1) - Limited by the number of bits, considering a finite loop bound.

**Space Complexity:** O(1) - Similarly, only a fixed number of integer variables are utilized.

These implementations demonstrate the power of bit manipulation in carrying out arithmetic operations such as addition, challenging the conventional thought of leveraging arithmetic operators in programming.

