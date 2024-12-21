# [Leetcode 201: Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)

## Approaches
1. [Brute Force Method](#approach-1-brute-force-method)
2. [Bit Manipulation](#approach-2-bit-manipulation)
3. [Position Shift](#approach-3-position-shift)

---

## Approach 1: Brute Force Method

### Intuition
The simplest way to tackle this problem is to iterate through the range from `m` to `n`, computing the bitwise AND for each number. This approach directly applies the definition of the bitwise AND operation over a range.

### Steps
1. Initialize a variable `result` with the value `m`.
2. Iterate through the numbers from `m+1` to `n`.
3. For each number, compute the bitwise AND with `result`.
4. Return `result` as the final answer.

### Code
```go
func rangeBitwiseAnd(m int, n int) int {
    result := m
    for i := m + 1; i <= n; i++ {
        // Compute the AND between result and the current number
        result &= i
        // If result becomes 0, break to optimize time
        if result == 0 {
            break
        }
    }
    return result
}
```

### Complexity Analysis
- **Time Complexity**: O(n-m), where `n` and `m` are the given bounds. We iterate through each number in the range.
- **Space Complexity**: O(1), as no additional data structures are used.

---

## Approach 2: Bit Manipulation

### Intuition
A more efficient approach involves exploiting properties of bitwise operations. Notice that when the number range increases, the number of trailing zeroes in the final result increases. This is because each bit changes at different times, starting from the least significant bit. If a bit in the final result is to be kept, it must remain constant across the whole range.

### Steps
1. Initialize `shift` to zero to count the number of times we have shifted `m` and `n`.
2. While `m` is not equal to `n`:
   - Right shift both `m` and `n`.
   - Increment the shift count.
3. Left shift `m` by the count of shifts to recover the common prefix. This is the answer.

### Code
```go
func rangeBitwiseAnd(m int, n int) int {
    shift := 0
    // Find common prefix of m and n
    for m < n {
        m >>= 1
        n >>= 1
        shift++
    }
    // Shift back the common prefix
    return m << shift
}
```

### Complexity Analysis
- **Time Complexity**: O(log(n)), because we are continuously halving `m` and `n`, similar to a binary search pattern.
- **Space Complexity**: O(1), as no extra data structures are used.

---

## Approach 3: Position Shift

### Intuition
The idea here is to focus on removing the least significant bits of `m` and `n` until they are the same. This is equivalent to finding a common prefix of the two numbers in their binary representation.

### Steps
1. Initialize a `count` of bits removed to zero.
2. While `m` and `n` differ:
   - Update both to their half (right shift).
   - Increment the `count`.
3. Once `m` equals `n`, reconstruct the number using `m` left shifted by the `count`.

### Code
```go
func rangeBitwiseAnd(m int, n int) int {
    count := 0
    for m != n {
        m >>= 1
        n >>= 1
        count++
    }
    return m << count
}
```

### Complexity Analysis
- **Time Complexity**: O(log(n)), since we are halving the numbers, which implies logarithmic complexity related to their binary representation.
- **Space Complexity**: O(1), as this method uses a constant amount of space.

---

By progressing from a brute-force approach to optimized bit manipulation methods, one can efficiently solve the problem and understand the intricacies of bitwise operations.

