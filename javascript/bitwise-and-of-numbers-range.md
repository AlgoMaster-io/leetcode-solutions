# [Leetcode Problem 201: Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches:
- [Brute Force Approach](#brute-force-approach)
- [Bit Shift Approach](#bit-shift-approach)

---

### Brute Force Approach

**Intuition:**

In this approach, we calculate the bitwise AND of each number in the range starting from `m` to `n`. This is a very straightforward approach where we iterate through every number and accumulate the bitwise AND result.

**Steps:**

1. Initialize a variable `result` with the value of `m`.
2. Iterate over the numbers from `m+1` to `n`.
3. For each number, update `result` to the bitwise AND of `result` and the current number.
4. Return the `result` at the end.

```javascript
function rangeBitwiseAnd(m, n) {
    // Initialize result with m
    let result = m;

    // Compute the AND for the entire range
    for (let i = m + 1; i <= n; i++) {
        result &= i;

        // Early stopping if result becomes zero, as AND with zero remains zero
        if (result === 0) {
            break;
        }
    }
    
    return result;
}
```

**Time Complexity:** O(n - m), because we iterate through every number in the range.

**Space Complexity:** O(1), as we are using a constant amount of extra space.

---

### Bit Shift Approach

**Intuition:**

The intuition here is based on the understanding that the bitwise AND of a range results in retaining only the leftmost bits (the common prefix) of all numbers in the range. Any differing parts will eventually resolve to zero. Hence, we seek to retain only the bits that remain consistent across `m` and `n`.

**Steps:**

1. Initialize a `shift` counter to count the number of shifts (or bits eliminated).
2. While `m` is not equal to `n`, right shift both `m` and `n`.
3. Increment the `shift` counter as shifting helps in aligning the numbers to eliminate the differing right side bits.
4. Left shift the final value of `m` back by `shift` to restore it to the range-bitwise-and value.
5. Return the left-shifted value as the result.

```javascript
function rangeBitwiseAnd(m, n) {
    let shift = 0;

    // Align m and n until they are equal
    while (m < n) {
        m >>= 1;
        n >>= 1;
        shift++;
    }
    
    // Shift back to the left to align into the correct range
    return m << shift;
}
```

**Time Complexity:** O(log n), as we continuously right shift, reducing the numbers.

**Space Complexity:** O(1), since no additional data structure is used.

---
Both approaches tackle the problem with different learnings; however, the Bit Shift approach is more optimal, especially when dealing with large ranges, due to its logarithmic complexity.

