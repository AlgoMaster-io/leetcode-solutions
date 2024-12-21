# [Leetcode 190: Reverse Bits](https://leetcode.com/problems/reverse-bits/)

## Approaches
1. [Bit Manipulation with Loop](#bit-manipulation-with-loop)
2. [Optimized Bit Manipulation with Precomputed Table](#optimized-bit-manipulation-with-precomputed-table)

### 1. Bit Manipulation with Loop

#### Intuition
The problem requires us to reverse the bits of a given 32 bits unsigned integer. The most basic solution to reverse bits is to iterate over each bit position from 0 to 31, pull out each bit, and then add it to the reversed position in the result integer.

#### Detailed Steps
1. Initialize a result integer to 0.
2. Loop over each bit position `i` from 0 to 31.
3. For each position:
    - Extract the ith bit from the input number.
    - Place this extracted bit at the correct position from the reverse end (31-i) in the result.
4. After the loop ends, the result will be the input integer with its bits reversed.

#### Time Complexity
- **Time**: O(1) - The loop runs 32 times which is a constant.
- **Space**: O(1) - We are using a constant amount of extra space.

```typescript
function reverseBits(n: number): number {
    let result = 0;
    
    for (let i = 0; i < 32; i++) {
        // Extract the ith bit from n
        const bit = (n >> i) & 1;
        // Reverse the bit's position and add it to result
        result |= bit << (31 - i);
    }
    
    return result >>> 0; // Ensure the result is treated as an unsigned 32-bit integer
}
```

### 2. Optimized Bit Manipulation with Precomputed Table

#### Intuition
This approach leverages a lookup table to reverse each byte. Since a byte consists of only 8 bits, we can pre-compute the reverse for every possible byte (0 to 255). This reduces the problem of reversing 32 bits to reversing four individual bytes, each of which can quickly be accessed via the table.

#### Detailed Steps
1. Precompute a table of size 256, where each index corresponds to a number between 0 and 255, and the value is the reverse of the bits of the index.
2. Split the input number into 4 bytes, reverse each byte using the precomputed table, and shift them to their correct positions to form the reversed 32-bit integer.

#### Time Complexity
- **Time**: O(1) - The number of operations is constant due to fixed shifts and lookups.
- **Space**: O(1) - There is a constant space overhead for the lookup table.

```typescript
function reverseBitsOptimized(n: number): number {
    const reverseByte = new Array(256).fill(0);

    // Precompute the reverse of each byte value
    for (let i = 0; i < 256; i++) {
        let rev = i;
        rev = ((rev & 0b11110000) >> 4) | ((rev & 0b00001111) << 4);
        rev = ((rev & 0b11001100) >> 2) | ((rev & 0b00110011) << 2);
        rev = ((rev & 0b10101010) >> 1) | ((rev & 0b01010101) << 1);
        reverseByte[i] = rev;
    }
    
    // Split n into bytes and reverse each
    let result = reverseByte[n & 0xff] << 24 |
                 reverseByte[(n >> 8) & 0xff] << 16 |
                 reverseByte[(n >> 16) & 0xff] << 8 |
                 reverseByte[(n >> 24) & 0xff];
                 
    return result >>> 0;
}
```

Each approach provides a complete encapsulation of strategies to reverse bits of a 32-bit unsigned integer, progressively moving from a basic understanding to a more optimized solution with precomputed values.

