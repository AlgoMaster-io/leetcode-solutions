# [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/)

## Approaches:
- [Approach 1: Loop and Bit Manipulation](#approach-1-loop-and-bit-manipulation)
- [Approach 2: Brian Kernighan's Algorithm](#approach-2-brian-kernighans-algorithm)
  
## Approach 1: Loop and Bit Manipulation
The simplest approach to count the number of '1' bits is to loop through each bit of the integer and check if it is '1'. This can be achieved efficiently using bitwise operations.

### Intuition
- We use a mask `00000001` to check each bit of the number. 
- For each bit position, we use the bitwise AND operator `&` to determine if the current bit in `n` is set (`1`).
- We then shift the mask left by one each time to move to the next bit.

### Detailed Steps:
1. Initialize a `mask` with the value `00000001` (binary), which means `1` in decimal.
2. Iterate over each of the 32 bits of an integer.
3. Use bitwise AND to check if the current bit in `n` is `1`.
4. If it is, increment the count.
5. Shift the mask to the left by 1 to move to the next bit.
6. Return the count of `1's`.

### Code:
```csharp
public class Solution {
    public int HammingWeight(uint n) {
        int count = 0;
        uint mask = 1;
        
        for (int i = 0; i < 32; i++) {
            // Use AND operation to check the ith bit
            if ((n & mask) != 0) {
                count++;
            }
            // Shift the mask to the left by 1 bit
            mask <<= 1;
        }
        
        return count;
    }
}
```

### Time Complexity:
- **O(1)**: Because the loop always runs 32 times, regardless of the input size.

### Space Complexity:
- **O(1)**: Only a constant amount of space is used.

## Approach 2: Brian Kernighan's Algorithm
Brian Kernighan’s Algorithm is a more efficient method to count the number of 1 bits. It works by flipping the least-significant 1-bit to 0 in each iteration.

### Intuition
- When subtracting `1` from a number, all bits after the rightmost `1` including the rightmost `1` flip.
- By performing an AND operation with `n` and `n - 1`, the rightmost `1` in `n` is turned to `0`.
- By repeatedly flipping the rightmost `1` bit to `0`, you can count how many `1` bits exist.

### Detailed Steps:
1. Initialize `count` as `0`.
2. While `n` is not `0`:
   - Increment the `count`.
   - Update `n` to `n & (n - 1)`, which removes the least significant `1`.
3. Return `count`.

### Code:
```csharp
public class Solution {
    public int HammingWeight(uint n) {
        int count = 0;
        
        while (n != 0) {
            count++;
            // Flip the least significant 1-bit in n
            n &= (n - 1);
        }
        
        return count;
    }
}
```

### Time Complexity:
- **O(k)**: Where `k` is the number of 1 bits in `n`. This is generally faster than the first approach for sparse bit strings with few `1`s.

### Space Complexity:
- **O(1)**: Only a constant amount of space is used.

Both approaches effectively solve the problem, but using Brian Kernighan's technique is generally more efficient for numbers with fewer '1' bits due to its reduction in loop iterations.

