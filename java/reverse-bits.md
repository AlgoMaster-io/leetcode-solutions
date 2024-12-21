You can access the problem on LeetCode using the following link: [Leetcode Problem 190: Reverse Bits](https://leetcode.com/problems/reverse-bits/)

### Approaches:

1. [Simple Bit Manipulation](#approach-1-simple-bit-manipulation)
2. [Efficient Bit Manipulation with Bit Masks and Shifts](#approach-2-efficient-bit-manipulation-with-bit-masks-and-shifts)

---

## Approach 1: Simple Bit Manipulation

### Intuition:
The problem of reversing bits can be solved by taking each bit from the rightmost side of the input number and placing it in the leftmost side of the result number. We need to iterate over all 32 bits since the function requires us to handle a 32-bit unsigned integer.

### Steps:
1. Initialize a result variable to zero to store the reversed bits.
2. Use a loop to process all 32 bits of the number.
3. In each iteration, check if the least significant bit of the number is 1.
4. If it is 1, set the corresponding bit in the result by using left shift on the result and then OR operation.
5. Right shift the input number to process the next bit.
6. After processing all 32 bits, the 'result' variable will contain the reversed bits.

### Code:

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;
        for (int i = 0; i < 32; i++) {
            // Left shift the result to make space for the next bit
            result <<= 1;
            // Check if the lowest bit of n is 1
            if ((n & 1) == 1) {
                result |= 1; // Set the corresponding bit in result
            }
            n >>= 1; // Right shift n to process the next bit
        }
        return result; // Return the reversed bits
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(32) = O(1), since we perform a constant amount of work (32 iterations).
- **Space Complexity**: O(1), since we use a constant amount of space.

---

## Approach 2: Efficient Bit Manipulation with Bit Masks and Shifts

### Intuition:
Instead of processing one bit at a time, we can optimize the operation by swapping bits in pairs, and then groups of four, eight, and so on. This allows more significant bits to be moved in fewer operations.

### Steps:
1. Use masks to isolate and swap bits at target positions.
2. Start by swapping adjacent pairs, followed by swapping nibbles (4-bit groups), then octets (8-bit groups), and finally by half-words (16-bit groups).
3. This approach exploits the property of bit manipulation using masks and shifts to achieve a similar result in fewer operations.

### Code:

```java
public class Solution {
    public int reverseBits(int n) {
        // Swap adjacent pairs
        n = ((n >>> 1) & 0x55555555) | ((n & 0x55555555) << 1);
        // Swap nibbles
        n = ((n >>> 2) & 0x33333333) | ((n & 0x33333333) << 2);
        // Swap bytes
        n = ((n >>> 4) & 0x0F0F0F0F) | ((n & 0x0F0F0F0F) << 4);
        // Swap two-byte long pairs
        n = ((n >>> 8) & 0x00FF00FF) | ((n & 0x00FF00FF) << 8);
        // Swap halves
        n = ((n >>> 16) & 0x0000FFFF) | ((n & 0x0000FFFF) << 16);
        return n;
    }
}
```

### Complexity Analysis:
- **Time Complexity**: O(1), since we're performing a fixed number of operations.
- **Space Complexity**: O(1), as only a constant amount of space is required.

In this more optimal solution, the use of bit masks allows us to group and move bits more efficiently, resulting in faster execution.

