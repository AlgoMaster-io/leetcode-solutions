# [Leetcode 190: Reverse Bits](https://leetcode.com/problems/reverse-bits/)

### Approaches:
- [Approach 1: Bit by Bit Reversal](#approach-1-bit-by-bit-reversal)
- [Approach 2: Optimized Reversal Using Bit Manipulation](#approach-2-optimized-reversal-using-bit-manipulation)

---

## Approach 1: Bit by Bit Reversal

### Intuition:
The simplest way to reverse the bits of a 32-bit integer is to iterate through each bit, extract it using bit manipulation, and place it in its reversed position in a new integer.

### Steps:
1. Start with an empty result integer initialized to zero.
2. For each bit position `i` from 0 to 31:
   - Extract the i-th bit from the original number.
   - Set the corresponding bit in the result (which is the (31-i)th position).
3. Shift the extracted bit to its new position and use a bitwise OR to set it in the result.
4. Return the result integer after processing all bits.

### Code:

```cpp
uint32_t reverseBits(uint32_t n) {
    uint32_t result = 0;
    for (int i = 0; i < 32; ++i) {
        // Extract the i-th bit from the right
        uint32_t bit = (n >> i) & 1;
        // Place it in the (31-i)th position
        result |= (bit << (31 - i));
    }
    return result;
}
```

### Time Complexity:
- O(32) = O(1): Since the number of bits is fixed (32 bits), the number of operations is constant.

### Space Complexity:
- O(1): No extra space is used beyond the auxiliary variables.

---

## Approach 2: Optimized Reversal Using Bit Manipulation

### Intuition:
Instead of processing individual bits, we can optimize the reversal by swapping bits in larger chunks. By using masks and shifts, we can swap bits in a divide-and-conquer manner.

### Steps:
1. Utilize mask-based swaps to switch bits in pairs, nibbles (4 bits), bytes (8 bits), and finally 16-bit halves.
2. Swap odd and even bits using a mask.
3. Swap consecutive pairs of bits.
4. Swap nibbles (4-bit blocks).
5. Swap bytes.
6. Swap 16-bit halves.

### Code:

```cpp
uint32_t reverseBits(uint32_t n) {
    // Swap odd and even bits
    n = ((n >> 1) & 0x55555555) | ((n & 0x55555555) << 1);
    // Swap consecutive pairs
    n = ((n >> 2) & 0x33333333) | ((n & 0x33333333) << 2);
    // Swap nibbles
    n = ((n >> 4) & 0x0F0F0F0F) | ((n & 0x0F0F0F0F) << 4);
    // Swap bytes
    n = ((n >> 8) & 0x00FF00FF) | ((n & 0x00FF00FF) << 8);
    // Swap 16-bit blocks
    n = (n >> 16) | (n << 16);
    return n;
}
```

### Time Complexity:
- O(1): Similar to the basic approach, operations are constant due to the fixed size of integer.

### Space Complexity:
- O(1): No extra space other than auxiliary variables.

---


