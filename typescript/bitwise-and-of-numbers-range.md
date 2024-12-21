# [Leetcode 201: Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches:

1. [Basic Iterative Approach](#basic-iterative-approach)
2. [Bit Manipulation - Right Shift Approach](#bit-manipulation---right-shift-approach)

---

### Basic Iterative Approach

#### Intuition:
The straightforward approach would involve iterating through each number from the start of the range to the end and performing a bitwise AND operation with a result variable. This approach is quite intuitive but isn't optimal for large ranges, as it involves iterating through each element of the range.

#### Approach:
1. Initialize a variable `result` with the value of `m`.
2. Iterate from `m + 1` to `n`, performing the bitwise AND operation between `result` and the current number `i`.
3. Return `result` after processing all numbers in the range.

```typescript
function rangeBitwiseAnd_basic(m: number, n: number): number {
    let result = m; // Start with the first number in the range

    for (let i = m + 1; i <= n; i++) {
        result &= i; // Perform bitwise AND with each number in the range

        // Early exit if result becomes 0, as any further AND operation will also be zero
        if (result === 0) {
            return 0;
        }
    }
    return result;
}
```

#### Time Complexity:
- **O(n - m)**: The loop runs `n - m` times.

#### Space Complexity:
- **O(1)**: No additional space other than loop variables and the result.

---

### Bit Manipulation - Right Shift Approach

#### Intuition:
When performing a bitwise AND operation over a range of numbers, bits that change will eventually result in zeros after a certain point. The key insight here is to find the common left prefix of `m` and `n` because any differing bits in this range will be wiped out to zero.

#### Approach:
1. Initialize a `shift` counter to count how many bits we have shifted.
2. Shift `m` and `n` to the right until they become equal.
3. The number of shifts required is stored in `shift`.
4. The common prefix is then shifted back left, filling zeros to get the result.
5. Return this computed result.

```typescript
function rangeBitwiseAnd_optimal(m: number, n: number): number {
    let shift = 0; // Count the number of right shifts needed to equalize m and n

    // Shift both m and n to the right until they are the same
    while (m < n) {
        m >>= 1;
        n >>= 1;
        shift++; // Keep track of how many shifts
    }

    // Shift the common prefix back to the left
    return m << shift;
}
```

#### Time Complexity:
- **O(log n)**: We reduce the problem size significantly by shifting, effectively depending on the number of bits.

#### Space Complexity:
- **O(1)**: Only a constant amount of space is used for variables.

In summary, the iterative approach provides a clear understanding of the problem but is inefficient for large ranges. The optimal bit manipulation approach uses the insight of common prefixes to efficiently compute the solution.

