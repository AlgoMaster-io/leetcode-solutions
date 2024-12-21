# [Leetcode 191: Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approaches
1. [Loop and Bit Manipulation](#loop-and-bit-manipulation)
2. [Bit Shift and Masking](#bit-shift-and-masking)
3. [Brian Kernighan’s Algorithm](#brian-kernighans-algorithm)

---

### 1. Loop and Bit Manipulation

#### Intuition
The problem asks for the count of '1' bits in the binary representation of a number. A straightforward method is to loop through each bit of the integer and determine if it's a '1'. This approach works by continuously checking the least significant bit and then shifting the bits to the right.

#### Detailed Steps
- Initialize a counter to zero.
- Iterate over all 32 bits of the number (since the problem involves 32-bit unsigned integers).
- At each step, check if the least significant bit (LSB) is '1', and if so, increment the counter.
- Right shift the number by one to check the next bit.
- Continue until all bits are processed.

#### Code
```javascript
var hammingWeight = function(n) {
    let count = 0;
    for (let i = 0; i < 32; i++) {
        // Check if the least significant bit is 1
        if ((n & 1) === 1) {
            count++;
        }
        // Right shift the bits to process the next bit in the next iteration
        n = n >>> 1;
    }
    return count;
};
```

#### Complexity
- **Time Complexity**: O(1), since the loop runs a fixed number of times (32).
- **Space Complexity**: O(1), only a few variables are used.

---

### 2. Bit Shift and Masking

#### Intuition
This approach relies on masking techniques. The idea is to use a mask of `1` and shift it left each time to check each bit of the number from right to left.

#### Detailed Steps
- Start with a mask of `1`.
- Iterate 32 times, and during each iteration:
  - Perform a bitwise AND between the number `n` and the mask. If the result is non-zero, it means the bit in `n` corresponding to the current mask is '1'.
  - Increment the count based on the result of the AND operation.
  - Shift the mask to the left by 1 bit to check the next position.
- The loop ensures each bit position is checked exactly once.

#### Code
```javascript
var hammingWeight = function(n) {
    let count = 0;
    let mask = 1;
    for (let i = 0; i < 32; i++) {
        // Check if the current bit position is 1
        if ((n & mask) !== 0) {
            count++;
        }
        // Move the mask to the left by 1 bit
        mask = mask << 1;
    }
    return count;
};
```

#### Complexity
- **Time Complexity**: O(1), constant operations with loop running 32 times.
- **Space Complexity**: O(1), constant space used.

---

### 3. Brian Kernighan’s Algorithm

#### Intuition
Brian Kernighan’s algorithm is efficient for counting set bits in an integer. It iteratively flips the least significant '1' bit of the number until the number becomes zero.

#### Detailed Steps
- Initialize a counter to zero.
- While `n` is not zero:
  - Perform `n & (n - 1)`, which flips the least significant '1' bit in `n` to '0'.
  - Increment the counter each time you perform the operation.
- The number of times this operation is performed gives the count of '1' bits.

#### Code
```javascript
var hammingWeight = function(n) {
    let count = 0;
    while (n !== 0) {
        // Flip the least significant '1' bit to '0'
        n = n & (n - 1);
        count++;
    }
    return count;
};
```

#### Complexity
- **Time Complexity**: O(k), where k is the number of '1' bits in the number. In the worst case, it could be 32 (all bits are '1').
- **Space Complexity**: O(1), constant space used.

---

