# [Counting Bits](https://leetcode.com/problems/counting-bits/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Dynamic Programming Approach - Using Last Set Bit](#dynamic-programming-approach---using-last-set-bit)
3. [Dynamic Programming Approach - Using Least Significant Bit](#dynamic-programming-approach---using-least-significant-bit)

---

### Brute Force Approach

#### Intuition
For each number from 0 to n, count the number of 1s in its binary representation individually. This can be done by using a loop to check each bit of each number.

#### Steps:
1. Initialize an array `res` of length `n+1` with all elements 0.
2. For every number `i` from 0 to `n`:
    - Initialize a counter `count` to 0.
    - Check each bit of `i`. If a bit is set (1), increment `count`.
    - Assign `count` to `res[i]`.

```typescript
function countBits(n: number): number[] {
    const res = new Array(n + 1).fill(0);

    for (let i = 0; i <= n; i++) {
        let count = 0;
        let num = i;

        // Count number of 1s in the binary representation of the number
        while (num > 0) {
            count += num & 1; // Increment count if the last bit is 1
            num >>= 1;        // Shift bits to right by 1
        }

        res[i] = count;
    }

    return res;
}
```

#### Time Complexity
- **O(n log n)**: We loop through each number from 0 to n, and for each number, we may have to check up to `log n` bits.
  
#### Space Complexity
- **O(n)**: We are storing the results in an array.

---

### Dynamic Programming Approach - Using Last Set Bit

#### Intuition
Exploit the relationship between numbers and their binary representations: The number of bits in `i` is 1 plus the number of bits in `i & (i-1)`, where `i & (i-1)` drops the last set bit from `i`.

#### Steps:
1. Initialize an array `res` of length `n+1` with all elements 0.
2. For every number `i` from 1 to `n`:
    - Use the relation: `res[i] = res[i & (i - 1)] + 1`.

```typescript
function countBits(n: number): number[] {
    const res = new Array(n + 1).fill(0);

    for (let i = 1; i <= n; i++) {
        res[i] = res[i & (i - 1)] + 1; // Use relation to count bits
    }

    return res;
}
```

#### Time Complexity
- **O(n)**: We calculate the number of bits for each number up to n.

#### Space Complexity
- **O(n)**: We are storing the results in an array.

---

### Dynamic Programming Approach - Using Least Significant Bit

#### Intuition
We can also utilize the approach where we derive the count of bits from `i` based on the less significant numbers. Every number `i` can be broken into two parts: its least significant bit and `i >> 1`. Thus, `res[i] = res[i >> 1] + (i & 1)`.

#### Steps:
1. Initialize an array `res` of length `n+1` with all elements 0.
2. For every number `i` from 1 to `n`:
    - Use the relation: `res[i] = res[i >> 1] + (i & 1)`.

```typescript
function countBits(n: number): number[] {
    const res = new Array(n + 1).fill(0);

    for (let i = 1; i <= n; i++) {
        res[i] = res[i >> 1] + (i & 1); // Calculate number of 1s using right shift and bitwise AND
    }

    return res;
}
```

#### Time Complexity
- **O(n)**: We calculate the number of bits for each number up to n.

#### Space Complexity
- **O(n)**: We are storing the results in an array.

---

