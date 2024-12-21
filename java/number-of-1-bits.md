# [191. Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approaches:
1. [Brute Force Approach](#approach-1)
2. [Optimized Bit Manipulation](#approach-2)

## Approach 1: Brute Force Approach

### Intuition:
Counting the number of 1 bits in an integer can be approached by iterating through each bit and checking whether it is 1. This can be efficiently done by using bit shifts.

### Approach:
1. Initialize a counter to keep track of the number of 1 bits.
2. Use a loop to iterate over each of the 32 bits of the integer.
3. Use a bitmask to isolate each bit and determine if it's 1.
4. For each bit that is 1, increment the counter.
5. Return the counter as the result.

### Code:
```java
public class Solution {
    // Function to count the number of 1 bits in an integer
    public int hammingWeight(int n) {
        int count = 0;
        // Iterate through all 32 bits of the integer
        for (int i = 0; i < 32; i++) {
            // Check if the least significant bit is 1
            if ((n & 1) == 1) {
                count++;
            }
            // Shift the bits of n to the right by 1 to check the next bit
            n >>= 1;
        }
        return count;
    }
}
```

### Time Complexity:
- O(32) = O(1): Since the integer is a fixed 32-bit value.
  
### Space Complexity:
- O(1): Only constant space is used.


## Approach 2: Optimized Bit Manipulation

### Intuition:
The brute force approach can be optimized by using a clever trick with bit manipulation. By repeatedly turning off the rightmost 1-bit, we can directly count the 1-bits without checking each individual bit.

### Approach:
1. Initialize a counter to track the number of 1 bits.
2. Use a loop to iterate as long as `n` is not zero.
3. For each iteration, perform a bit operation `n = n & (n - 1)`, which turns off the rightmost bit of 1.
4. Increment the counter for each operation until `n` becomes zero.
5. Return the final count.

### Code:
```java
public class Solution {
    // Function to count the number of 1 bits in an integer
    public int hammingWeight(int n) {
        int count = 0;
        // Iterate until n becomes zero
        while (n != 0) {
            // Perform n & (n - 1) to remove the rightmost 1-bit
            n = n & (n - 1);
            // Increment the counter for each 1-bit removed
            count++;
        }
        return count;
    }
}
```

### Time Complexity:
- O(k): k is the number of 1-bits in the integer. In the worst case, k can be up to 32 for a 32-bit integer.

### Space Complexity:
- O(1): Only constant space is used. 

This optimized solution is much faster than counting through all bits, especially for numbers with a small number of 1-bits.

