# [Leetcode 338: Counting Bits](https://leetcode.com/problems/counting-bits/)

## Approaches:
- [Brute Force](#approach-1-brute-force)
- [Dynamic Programming (Popcount)](#approach-2-dynamic-programming-popcount)
- [Dynamic Programming (Least Significant Bit)](#approach-3-dynamic-programming-least-significant-bit)
  
## Approach 1: Brute Force

### Intuition
For each number `i` from 0 to `n`, compute the number of 1s in its binary representation. This can be done by converting the number to a binary string and counting the 1s.

### Steps
1. Initialize an empty result array `res` of size `n+1`.
2. For each number `i` from `0` to `n`:
   - Convert `i` to its binary string using `i.toString(2)`.
   - Split the binary string and count the number of `'1'`.
   - Store this count in `res[i]`.
3. Return the result array `res`.


### Code
```javascript
function countBits(n) {
    let res = new Array(n + 1);
    for (let i = 0; i <= n; i++) {
        // Convert number to binary string and count 1s
        res[i] = i.toString(2).split('0').join('').length;
    }
    return res;
}
```

### Complexity
- **Time Complexity**: O(n * k), where `n` is the input number and `k` is the average number of bits in binary representation.
- **Space Complexity**: O(n), since we store results for each number from 0 to `n`.

---

## Approach 2: Dynamic Programming (Popcount)

### Intuition
The number of 1s in binary for an integer `i` can be determined using previously computed results. The number of 1 bits for `i` is the number of 1 bits in `i>>1` plus the least significant bit of `i`.

### Steps
1. Create an array `res` of size `n + 1`.
2. Set `res[0]` to `0` since `0` has zero 1s.
3. Iterate through each number `i` from `1` to `n`:
   - Calculate `res[i] = res[i >> 1] + (i & 1)`.
     - `i >> 1` effectively divides `i` by `2`.
     - `(i & 1)` checks if `i` is odd (`1`) or even (`0`).
4. Return the result array `res`.

### Code
```javascript
function countBits(n) {
    let res = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        // Use the right shift and evaluate its smallest bit
        res[i] = res[i >> 1] + (i & 1);
    }
    return res;
}
```

### Complexity
- **Time Complexity**: O(n), because we compute the result for each number up to `n` in constant time.
- **Space Complexity**: O(n), due to the result array storing counts for each number.

---

## Approach 3: Dynamic Programming (Least Significant Bit)

### Intuition
A more optimal solution similar to the previous one but approaches it differently:
- Use the least significant set bit to determine the number of 1s.
- The formula gets simplified due to one particular bit contributing to the difference between consecutive numbers.

### Steps
1. Initialize an array `res` with size `n + 1`.
2. Set `res[0]` to `0`.
3. Iterate from `1` to `n` using index `i`:
   - Calculate `res[i] = res[i & (i - 1)] + 1`.
     - `i & (i - 1)` turns off the least significant bit of `i`.
4. Return the result array `res`.

### Code
```javascript
function countBits(n) {
    let res = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        // Use the relationship between the current number and the number formed by removing the least significant bit
        res[i] = res[i & (i - 1)] + 1;
    }
    return res;
}
```

### Complexity
- **Time Complexity**: O(n), as each number's bit count derives from a simple operation on its index.
- **Space Complexity**: O(n), for storing the number of 1s for each integer.

