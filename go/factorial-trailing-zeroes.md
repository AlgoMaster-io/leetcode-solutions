# [Leetcode 172: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Approaches
1. [Basic Approach: Calculate Factorial and Count Zeroes](#basic-approach-calculate-factorial-and-count-zeroes)
2. [Optimized Approach: Count Factors of 5](#optimized-approach-count-factors-of-5)

## Basic Approach: Calculate Factorial and Count Zeroes

### Intuition
The trailing zeroes in a factorial result from the factors of 10 in the product. A factor of 10 is made from a pair of 5 and 2. In most factorials, the number of twos is more than the number of fives, so the number of trailing zeros is determined by the number of fives.

In this basic approach, we calculate the factorial of a number directly and then count the number of trailing zeroes. This approach quickly becomes inefficient for large numbers due to the rapid growth in size of factorials.

### Code
```go
import "math/big"

func trailingZeroes(n int) int {
    // Calculate factorial using big integers since factorials grow very large
    factorial := big.NewInt(1)
    for i := 2; i <= n; i++ {
        factorial.Mul(factorial, big.NewInt(int64(i)))
    }
    
    // Convert the factorial to string to easily count trailing zeroes
    zeroCount := 0
    for _, ch := range factorial.String() {
        if ch == '0' {
            zeroCount++
        } else {
            zeroCount = 0 // reset count if a non-zero digit appears
        }
    }
    
    return zeroCount
}
```

### Time Complexity
- O(n): Calculating the factorial iterates through numbers from 1 to n.
- The space required is also significant due to the large size of numbers involved.

### Space Complexity
- O(1): Apart from the storage of the factorial, which is handled using a `big.Int`, additional space usage is minimal.

## Optimized Approach: Count Factors of 5

### Intuition
Rather than calculating the factorial, we recognize that trailing zeroes in a factorial number of n arise solely from the contributions of factor pair (2, 5) and mainly by the number of times 5 can be a divisor up to n. Therefore, we only need to count multiples of 5!

For any given number n:
- Count the number of 5's in the factors (e.g., 5, 25, 125, etc.)

### Code
```go
func trailingZeroes(n int) int {
    zeroCount := 0
    // Count factors of 5
    for n > 0 {
        n /= 5
        zeroCount += n
    }
    return zeroCount
}
```

### Time Complexity
- O(log₅n): Each division reduces the size by a factor of 5, which logarithmically reduces the number of calculations needed.

### Space Complexity
- O(1): Space usage is constant, requiring only a few variables regardless of input size.

