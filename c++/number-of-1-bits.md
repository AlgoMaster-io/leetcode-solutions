# [Leetcode 191: Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Optimized Approach Using Brian Kernighan's Algorithm](#optimized-approach-brian-kernighan)

## Brute Force Approach

### Intuition
The brute force approach involves checking each bit one-by-one to see if it is a '1'. We will right shift the number until it becomes zero, checking after each shift whether the least significant bit (LSB) is '1'. To isolate the LSB, we can use a bitwise AND operation with `1`.

### Steps:
1. Initialize a counter to count the number of '1' bits.
2. Loop through each bit of the integer:
   - Check if the current LSB is '1' by performing `n & 1`.
   - Right shift the number `n` by 1 to check the next bit.
3. Continue the loop until `n` becomes zero.
4. Return the counter.

### Time Complexity
- O(32) for a 32-bit integer, i.e., O(1), because we perform a fixed number of operations based on the number of bits, not the value of `n`.

### Space Complexity
- O(1), as we use a constant amount of extra space.

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n) {
            // Check if the last bit is 1
            count += (n & 1);
            // Right shift the number by 1 to check the next bit
            n >>= 1;
        }
        return count;
    }
};
```

## Optimized Approach: Brian Kernighan's Algorithm

### Intuition
Brian Kernighan's algorithm improves upon the previous method by performing fewer iterations. Instead of checking each bit, this method lowers the number of set bits directly until `n` becomes zero. We repeatedly flip the least significant bit that is set to zero, using `n & (n - 1)` to eliminate the rightmost '1' bit.

### Steps:
1. Initialize a counter to count the number of '1' bits.
2. While `n` is not zero:
   - Use the operation `n = n & (n - 1)` to turn off the rightmost '1'.
   - Increment the counter.
3. Return the counter.

### Time Complexity
- O(k), where `k` is the number of '1' bits. This is better than O(32) if the number of set bits is small.

### Space Complexity
- O(1), as we use a constant amount of extra space.

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n) {
            // Remove the rightmost '1' bit
            n = n & (n - 1);
            // Increment the counter for each '1' removed
            count++;
        }
        return count;
    }
};
```

Both approaches provide a reliable method to count the number of `1` bits in a 32-bit unsigned integer, but Brian Kernighan's algorithm is more efficient when the number of set bits is low.

