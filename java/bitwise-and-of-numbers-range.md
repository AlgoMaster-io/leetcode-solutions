# [Leetcode Problem: 201. Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches
- [Approach 1: Iterative Bitwise AND](#approach-1-iterative-bitwise-and)
- [Approach 2: Bit Manipulation - Common Prefix](#approach-2-bit-manipulation-common-prefix)

## Approach 1: Iterative Bitwise AND

### Intuition
The simplest method is to calculate the bitwise AND for every number in the range [m, n]. This approach guarantees that we consider every number but can result in unnecessary computations. The key observation in this approach is that any bit position that becomes zero remains zero for all subsequent numbers in the range.

### Algorithm
1. Initialize a result variable with the value of `m`.
2. Iterate through numbers from `m + 1` to `n`.
3. Perform a bitwise AND operation between the result and the current number.
4. Return the result.

This approach iterates over the range and progressively accumulates the AND result. However, this method can be quite inefficient for large ranges.

### Code
```java
public int rangeBitwiseAnd(int m, int n) {
    int result = m;
    // Iterate over each number in the range [m, n]
    for (int i = m + 1; i <= n; i++) {
        // Perform a bitwise AND with the current number
        result &= i;
        // Early termination if result is already zero
        if (result == 0) {
            break;
        }
    }
    return result;
}
```

### Complexity Analysis
- **Time Complexity:** O(n - m), where n and m are the given range bounds. This can be very large if the range is big.
- **Space Complexity:** O(1) as we are using a constant amount of extra space.

## Approach 2: Bit Manipulation - Common Prefix

### Intuition
In any range of numbers where the upper and lower bounds differ, certain trailing bits in the binary representation will vary. We can track how much both numbers need to be right-shifted until they match, effectively finding a common prefix. This prefix will form the result since any bits that remain the same between m and n should dominate the final AND operation.

### Algorithm
1. Initialize a shift counter to 0.
2. While m is not equal to n, right shift both m and n.
3. Increment the shift counter each time you right shift.
4. The result is the common prefix left-shifted back to its original position.

This approach efficiently finds the common range prefix, highlighting where numbers differ.

### Code
```java
public int rangeBitwiseAnd(int m, int n) {
    int shift = 0;
    // Shift both m and n until they are the same
    while (m < n) {
        m >>= 1;
        n >>= 1;
        shift++;
    }
    // Shift the common prefix back to its original position
    return m << shift;
}
```

### Complexity Analysis
- **Time Complexity:** O(log n), where n is the upper bound of the range. We are finding the point of divergence in bits.
- **Space Complexity:** O(1) as we use a constant amount of extra space.

By using bit manipulation, we efficiently determine the shared bits across the range, leading to a much more optimal solution for large ranges.

