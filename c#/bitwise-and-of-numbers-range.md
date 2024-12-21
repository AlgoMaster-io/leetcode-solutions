# [Bitwise AND of Numbers Range - LeetCode](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Bitwise Manipulation](#approach-2-bitwise-manipulation)

## Approach 1: Brute Force

### Intuition
In the brute force approach, we perform the bitwise AND operation on all numbers from `m` to `n`. Although this approach is straightforward and easy to implement, it is not efficient for large ranges because it examines each number in the given range.

### Algorithm
1. Initialize a variable `result` with `m` as its initial value.
2. Loop through each number `i` from `m + 1` to `n`.
3. Perform the bitwise AND operation between `result` and `i`, and store it back to `result`.
4. If at any point `result` becomes zero, break the loop early since further operations will also result in zero.
5. Return the `result`.

### Time Complexity
- O(n-m): Potentially very large when `n` is significantly bigger than `m`.

### Space Complexity
- O(1): Only constant space is needed.

```csharp
public int RangeBitwiseAnd(int m, int n) {
    int result = m; // Start with the first number in the range
    for (int i = m + 1; i <= n; i++) {
        result &= i; // Perform bitwise AND on each number in the range
        if (result == 0) { // If result is zero, break early as further ANDs will also be zero
            break;
        }
    }
    return result;
}
```

## Approach 2: Bitwise Manipulation

### Intuition
A more optimal approach involves examining the binary representation of numbers and exploiting the property that once a bit becomes zero in an AND operation, it will remain zero. The idea is to shift both numbers `m` and `n` to the right until they become equal, keeping track of how many shifts were made. The common left prefix after this process is the bitwise AND result for all numbers in the range.

### Algorithm
1. Initialize a variable `shift` to zero to count how many shifts are needed.
2. While `m` is not equal to `n`, right shift both `m` and `n` by 1. Increment the `shift` counter.
3. After the loop ends, `m` and `n` have been reduced to their common left prefix.
4. Shift `m` back to the left by `shift` times to find the result.
5. Return `m`.

### Time Complexity
- O(log(max(m, n))): The number of shifts needed depends on the number of bits in the largest number.

### Space Complexity
- O(1): Only constant space is needed.

```csharp
public int RangeBitwiseAnd(int m, int n) {
    int shift = 0; // Counter for how many shifts we perform

    // Shift both numbers until they become equal
    while (m < n) {
        m >>= 1; // Shift right to find common prefix
        n >>= 1;
        shift++; // Keep track of the number of shifts
    }

    // Shift the common prefix back to the left by the number of shifts
    return m << shift; 
}
```
This bitwise manipulation approach efficiently narrows down the common left prefix in binary, leading to a much faster solution for large ranges.

