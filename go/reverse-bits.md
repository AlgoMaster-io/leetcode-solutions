# [Reverse Bits](https://leetcode.com/problems/reverse-bits/)

## Approaches
- [Approach 1: Brute Force Approach](#approach-1-brute-force)
- [Approach 2: Bit Manipulation](#approach-2-bit-manipulation)
- [Approach 3: Optimized Bit Manipulation](#approach-3-optimized-bit-manipulation)

### Approach 1: Brute Force

**Intuition:**

The simplest way to reverse bits is to repeatedly extract the least significant bit from the input number and construct the reversed result by shifting previously extracted bits left and adding the new bit.

**Steps:**

1. Initialize the result variable with 0.
2. Iterate over each bit (32 times, assuming 32-bit integer).
3. For each iteration:
   - Extract the least significant bit of `n` using `n & 1`.
   - Add this bit to the result by shifting the current result to the left and adding the extracted bit.
   - Right shift `n` to get the next bit in the next iteration.
4. Return the result.

**Time Complexity:** \(O(1)\) because we only have a fixed number of 32 iterations.

**Space Complexity:** \(O(1)\) since no additional space is used.

```go
func reverseBits(n uint32) uint32 {
    var result uint32 = 0
    for i := 0; i < 32; i++ {
        // Extract the least significant bit and add it to the result
        result = (result << 1) | (n & 1)
        // Right shift n to move to the next bit
        n >>= 1
    }
    return result
}
```

### Approach 2: Bit Manipulation

**Intuition:**

Use a bit manipulation technique to reverse the bits efficiently rather than manually inspecting each bit. By carefully shifting and masking the input, we can swap parts of the number.

**Steps:**

1. Swap each pair of bits.
2. Swap each pair of 2-bit groups.
3. Swap each pair of 4-bit groups.
4. Continue the process up to 16-bit groups.
5. All the swaps appropriately reverse the bits.

**Time Complexity:** \(O(1)\) due to the constant number of swaps.

**Space Complexity:** \(O(1)\) as we do not use additional space.

```go
func reverseBits(n uint32) uint32 {
    n = (n >> 1 & 0x55555555) | (n & 0x55555555) << 1
    n = (n >> 2 & 0x33333333) | (n & 0x33333333) << 2
    n = (n >> 4 & 0x0f0f0f0f) | (n & 0x0f0f0f0f) << 4
    n = (n >> 8 & 0x00ff00ff) | (n & 0x00ff00ff) << 8
    n = (n >> 16 & 0x0000ffff) | (n & 0x0000ffff) << 16
    return n
}
```

### Approach 3: Optimized Bit Manipulation

**Intuition:**

Divide the bit-reversal problem into a set of smaller sub-problems using a technique that leverages efficient memory and processing time using lookup tables. This involves pre-computing reversed results for smaller sets of bits and combining them.

**Steps:**

1. Pre-compute a table for reversing 8 bits.
2. Use the table to reverse the input 32-bit number using the pre-computed values for each byte.

**Time Complexity:** \(O(1)\) as only fixed operations are used due to lookup.

**Space Complexity:** \(O(1)\) excluding the space for the lookup table, which is also small and constant.

```go
var lookupTable = [256]uint32{
    0x00, 0x80, 0x40, 0xC0, 0x20, 0xA0, 0x60, 0xE0,
    // ... initialize all values ...
    // This is a partial initialization. In practice, fill up according to the reversed bits.
}

func reverseBits(n uint32) uint32 {
    return (lookupTable[n&0xFF] << 24) |
           (lookupTable[(n>>8)&0xFF] << 16) |
           (lookupTable[(n>>16)&0xFF] << 8) |
           (lookupTable[(n>>24)&0xFF])
}
```

Note: For this method, you would need to properly initialize the `lookupTable` with the reversed value of each 8-bit integer (0-255) for a fully working implementation.

