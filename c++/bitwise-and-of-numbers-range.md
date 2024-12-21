# [201. Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches
1. [Basic Approach: Iterative](#iterative-approach)
2. [Optimized Approach: Clear Least Significant Bits](#optimized-clear-bits-approach)

---

## 1. Iterative Approach

### Intuition:

The problem requires us to find the bitwise AND of all numbers in the given range `[m, n]`. The naive way would be to iterate from `m` to `n` and keep performing the AND operation. This approach is straightforward but not optimal for large ranges.

### Approach:

- Start with a variable `result` initialized to `m`.
- Iterate through each number from `m+1` to `n`.
- Perform `result = result & i`.
- This is computationally expensive with larger ranges since it requires iterating over every single number.

### Code:

```cpp
int rangeBitwiseAnd(int m, int n) {
    // Start with result as m
    int result = m;
    // Iterate through all numbers from m+1 to n
    for (int i = m + 1; i <= n; ++i) {
        // Perform bitwise AND of result with the current number
        result &= i;
        // Early stopping if result becomes zero
        if (result == 0) {
            break;
        }
    }
    return result;
}
```

### Complexity:

- **Time Complexity**: O(n-m), as we iterate over all numbers in the range.
- **Space Complexity**: O(1), since no extra space is used aside from a few variables.

---

## 2. Optimized Clear Bits Approach

### Intuition:

When you apply bitwise AND on numbers that differ by a large margin, the differing least significant bits turn zero. In other words, we should focus on the common prefix of `m` and `n`. The common prefix remains unchanged when differing least significant bits disappear.

### Approach:

- Shift `m` and `n` to the right until both numbers are equal. This indicates that all differing bits have been removed.
- Count the number of shifts needed.
- Once the loop exits, `m` (or `n`, since both are equal) contains only the common prefix.
- Shift `m` back to the left by the number of shifts counted to get the result.

### Code:

```cpp
int rangeBitwiseAnd(int m, int n) {
    // Initialize a counter for the number of shifts
    int shift = 0;
    // Find the common prefix
    while (m < n) {
        // Right shift both m and n
        m >>= 1;
        n >>= 1;
        // Increase the shift count
        ++shift;
    }
    // Left shift to restore the original bit length with zeroes
    return m << shift;
}
```

### Complexity:

- **Time Complexity**: O(log n), because we only shift as many times as there are bits in the numbers.
- **Space Complexity**: O(1), as it only uses a few integer variables.

This optimized approach efficiently isolates the common prefix of `m` and `n` to efficiently find the bitwise AND of the range.

