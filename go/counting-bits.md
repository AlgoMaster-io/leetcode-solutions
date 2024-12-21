# [Leetcode 338: Counting Bits](https://leetcode.com/problems/counting-bits/)

## Solutions
- [Approach 1: Iteration with Pop Count](#approach-1-iteration-with-pop-count)
- [Approach 2: Dynamic Programming - Counting Bits with Offset](#approach-2-dynamic-programming-counting-bits-with-offset)

### Approach 1: Iteration with Pop Count

**Intuition:**

A straightforward approach to solve this problem is to iterate from `0` to `n` and, for each number, count the number of `1` bits (also known as "pop count"). The pop count of a number can be calculated by continuously right-shifting the number and counting the number of times `num & 1` is `1`.

**Time Complexity:** `O(n * number of bits in the largest number)`  
**Space Complexity:** `O(1)` additional space besides the result

```go
func countBits(num int) []int {
    // Initialize a result array of size num+1.
    res := make([]int, num + 1)

    // Iterate over each number from 0 to num
    for i := 0; i <= num; i++ {
        count := 0 // Count of 1s in the binary representation of i
        n := i
        // Repeat until n becomes zero
        for n > 0 {
            // Add 1 to count if the least significant bit of n is 1.
            count += n & 1
            // Right shift n by 1 to check the next bit
            n >>= 1
        }
        // Store the count of 1s in res[i]
        res[i] = count
    }
    return res
}
```

### Approach 2: Dynamic Programming - Counting Bits with Offset

**Intuition:**

The number of `1` bits in any number `i` can be related to the number of `1` bits in numbers less than `i`. Specifically, the key observation is that the number of `1`s in `i` is equal to the number of `1`s in `i // 2` plus `i % 2`. This is because:
- `i // 2` is equivalent to `i >> 1`, which is `i` with its last bit removed.
- Adding the last bit (`i % 2`) accounts for the parity (odd/even nature of `i`).

`res[i] = res[i >> 1] + (i & 1)`

This allows us to build up the solution incrementally using previously computed results.

**Time Complexity:** `O(n)`
**Space Complexity:** `O(n)`

```go
func countBits(num int) []int {
    // Initialize a result array of size num+1.
    res := make([]int, num + 1)

    // Iterate over each number from 1 to num
    for i := 1; i <= num; i++ {
        // Calculate the number of 1 bits using previously computed results
        res[i] = res[i >> 1] + (i & 1)
    }
    return res
}
```

This second approach efficiently builds upon previously calculated subproblems, resulting in a fast and space-efficient solution compared to the basic iterative bit-counting method.

