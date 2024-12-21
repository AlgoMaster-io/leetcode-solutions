# [Reverse Bits](https://leetcode.com/problems/reverse-bits/)

## Approaches
- [Basic Bit Manipulation](#basic-bit-manipulation)
- [Optimal Approach with Bitwise Shift and Mask](#optimal-approach-with-bitwise-shift-and-mask)

### Basic Bit Manipulation

This naive approach involves extracting each bit from the input number, reversing its position, and constructing a new reversed bits number.

#### Intuition
For a 32-bit unsigned integer, we need to reverse the order of these bits. We'll perform this by iterating through each bit position of the input number, from least significant to most significant, and setting them in the correct reverse position in the result number.

The basic idea is to:
1. Initialize a `result` variable to zero.
2. Iterate through each bit of the input number.
3. At each step, check if the least significant bit is set.
4. If set, update the corresponding position in the reversed result.
5. Shift the input number to the right to process the next bit.
6. In parallel, shift the result to the left to prepare for setting the next bit.

#### Python Code
```python
def reverseBits(n: int) -> int:
    result = 0
    
    for i in range(32):
        # Extract the least significant bit of n
        bit = n & 1
        
        # Shift the extracted bit to its correct reversed position
        result = (result << 1) | bit
        
        # Prepare n for the next iteration by shifting right
        n >>= 1
    
    return result
```

#### Complexity Analysis
- **Time Complexity:** O(1), due to the fixed number of 32 iterations regardless of the number value.
- **Space Complexity:** O(1), since only a constant amount of space is used.

### Optimal Approach with Bitwise Shift and Mask

The optimal solution streamlines the bit reversal using the same logic but emphasizes how masks and shifts are applied for efficient computation.

#### Intuition
The optimal approach follows similar bit manipulation logic but minimizes operations using masking and bitwise shifts more succinctly. By leveraging bit operations, we can ensure cleaner and potentially faster processing at each iteration.

1. We perform the calculations using shifts and bit setting operations, which is inherently efficient due to low-level manipulations.
2. The combined operations in setting bits in the result, after ensuring correct positioning, is where we can observe potential auto-optimizations on some compilers.

#### Python Code
```python
def reverseBits(n: int) -> int:
    result = 0
    for _ in range(32):
        # Combine shift and masking operations
        result = (result << 1) | (n & 1)
        n >>= 1  # Move to process next bit
        
    return result
```

#### Complexity Analysis
- **Time Complexity:** O(1), each bit is processed in constant time.
- **Space Complexity:** O(1), no additional space is required beyond the primary variables.

Both approaches-involving bitwise operations are ultimately optimal for the problem context, but clear structuring of code can make it more robust and easier to understand at scale with larger bit manipulations if extended.

