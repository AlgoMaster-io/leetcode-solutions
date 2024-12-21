# [Leetcode 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approaches
- [Iterative Approach](#iterative-approach)
- [Recursive Approach](#recursive-approach)
- [Optimized Recursive Approach using Exponentiation by Squaring](#optimized-recursive-approach-using-exponentiation-by-squaring)

### Iterative Approach

**Intuition**:
The basic idea is to multiply `x` a total of `n` times. For a negative exponent, compute the positive exponent and finally take the reciprocal. This approach simply iterates, so it’s straightforward but not the most efficient.

```go
func myPow(x float64, n int) float64 {
    N := n
    if N < 0 {
        x = 1 / x  // Handle negative power by reciprocating the base
        N = -N
    }
    result := 1.0
    for N > 0 {
        result *= x  // Multiply the base x repeatedly for N times
        N--
    }
    return result
}
```

**Time Complexity**: O(N), where N is the absolute value of the exponent n.

**Space Complexity**: O(1) – we only use a constant amount of extra space.

### Recursive Approach

**Intuition**:
A recursive method reduces the problem size with each call. We compute half of the result recursively, then multiply it by itself. This method results in a more structured and elegant recursion at the cost of potential stack overflow issues with large n.

```go
func myPow(x float64, n int) float64 {
    if n == 0 {
        return 1 // The base case for recursion
    }
    if n < 0 {
        x = 1 / x  // Handle negative power by converting the base
        n = -n
    }
    return x * myPow(x, n-1) // Recursively multiply until n reaches zero
}
```

**Time Complexity**: O(N), where N is the absolute value of the exponent n.

**Space Complexity**: O(N) due to the recursive stack space in case of large n.

### Optimized Recursive Approach using Exponentiation by Squaring

**Intuition**:
Exponentiation by squaring is a faster approach to calculating powers. The idea is to compute `x^n` using the results of `x^(n/2)`. If n is even, `x^n = (x^(n/2))^2`. If n is odd, `x^n = x * (x^(n/2))^2`. This reduces the number of multiplications significantly, making the algorithm very efficient.

```go
func myPow(x float64, n int) float64 {
    if n == 0 {
        return 1 // Any number to the power of 0 is 1
    }
    if n < 0 {
        x = 1 / x  // Handle negative power by reciprocating the base
        n = -n
    }
    half := myPow(x, n/2) // Recursively compute half power
    if n%2 == 0 {
        return half * half  // If n is even
    } else {
        return x * half * half  // If n is odd
    }
}
```

**Time Complexity**: O(log N), where N is the absolute value of the exponent n. This is much more efficient due to the halving process.

**Space Complexity**: O(log N) due to the recursive stack space.

These solutions provide a comprehensive view of different methodologies to solve the `Pow(x, n)` problem, highlighting the evolution from a simple iterative approach to a sophisticated recursive method with a focus on optimization.

