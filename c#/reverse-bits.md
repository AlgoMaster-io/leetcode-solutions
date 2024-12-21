# [Leetcode 190: Reverse Bits](https://leetcode.com/problems/reverse-bits/)

## Approaches
- [Approach 1: Bit by Bit Reversal](#approach-1-bit-by-bit-reversal)
- [Approach 2: Optimized Bit Manipulation](#approach-2-optimized-bit-manipulation)

## Approach 1: Bit by Bit Reversal

### Intuition
The idea is to extract each bit from the input number and append it to the reversed result. Since a 32-bit number is given, we will iterate 32 times, shifting bits accordingly. This approach involves taking each bit and placing it in its reverse position.

### Steps
1. Initialize a `result` variable to 0 which will hold the reversed bits.
2. Loop through each of the 32 bits:
   - Extract the least significant bit from `n` using `n & 1`.
   - Shift the `result` to the left by one to make room for this new bit.
   - Append the extracted bit to `result`.
   - Shift `n` to the right by one to process the next bit in the next iteration.
3. Return the `result` after the loop completes.

### Time Complexity
- O(1): Since we always loop through 32 bits regardless of input value, the time complexity is constant.

### Space Complexity
- O(1): No additional data structures are used besides integer variables.

### Code
```csharp
public uint reverseBits(uint n) {
    uint result = 0;
    for (int i = 0; i < 32; i++) {
        // Extract the least significant bit of n
        uint bit = n & 1;
        
        // Shift result to the left to make room for the new bit
        result <<= 1;
        
        // Insert the extracted bit into result
        result |= bit;
        
        // Shift n to the right to process the next bit
        n >>= 1;
    }
    return result;
}
```

## Approach 2: Optimized Bit Manipulation

### Intuition
Rather than iterate over each bit individually, you can exploit symmetry and swap bits in larger chunks. This approach significantly speeds up operations through fixed bit-level manipulations. Use precomputed masks to selectively swap bits across multiple stages.

### Steps
1. Swap consecutive pairs of bits.
2. Swap 2-bit sets in the result.
3. Swap 4-bit sets in the result.
4. Continue exchanging progressively larger sets (8-bit, 16-bit) using bit masks.
5. This efficient swapping minimizes operations to reach the reversed 32-bit number quickly.

### Time Complexity
- O(1): The operations are fixed in number and sequence, hence constant time complexity.

### Space Complexity
- O(1): No additional space used beyond integer variables.

### Code
```csharp
public uint reverseBits(uint n) {
    // Swap consecutive pairs of bits
    n = ((n >> 1) & 0x55555555) | ((n & 0x55555555) << 1);
    // Swap consecutive 2-bit pairs
    n = ((n >> 2) & 0x33333333) | ((n & 0x33333333) << 2);
    // Swap consecutive 4-bit groups
    n = ((n >> 4) & 0x0F0F0F0F) | ((n & 0x0F0F0F0F) << 4);
    // Swap consecutive 8-bit groups
    n = ((n >> 8) & 0x00FF00FF) | ((n & 0x00FF00FF) << 8);
    // Swap consecutive 16-bit groups
    n = (n >> 16) | (n << 16);
    
    return n;
}
```

In this approach, we execute a sequence of bit manipulations that swap progressively larger groups of bits. These swaps are designed to halve the problem size at each step, providing an efficient means to reverse the entire bit string.

