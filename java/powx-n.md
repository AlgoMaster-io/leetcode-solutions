# [Leetcode 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approaches

1. [Iterative Multiplication](#iterative-multiplication)
2. [Recursive Approach with Divide and Conquer](#recursive-approach-with-divide-and-conquer)
3. [Iterative Binary Exponentiation (Optimal)](#iterative-binary-exponentiation-optimal)

### Iterative Multiplication

#### Intuition

The simplest way to calculate `x^n` is to multiply `x` by itself `n` times. While this approach is straightforward, it's very inefficient for large values of `n` due to its linear complexity. Also, handling negative powers involves taking the reciprocal of positive power results.

#### Code

```java
public class Solution {
    public double myPow(double x, int n) {
        // Handle the case for negative exponents
        if (n < 0) {
            x = 1 / x;
            n = -n;
        }
        double result = 1.0;
        // Multiply x by itself n times
        for (int i = 0; i < n; i++) {
            result *= x;
        }
        return result;
    }
}
```

#### Time Complexity

- **O(n)**: The loop runs `n` times.

#### Space Complexity

- **O(1)**: No additional space is used.

---

### Recursive Approach with Divide and Conquer

#### Intuition

The key observation is that `x^n` can be broken down recursively:
- If `n` is even, `x^n = x^(n/2) * x^(n/2)`
- If `n` is odd, `x^n = x * x^(n//2) * x^(n//2)`

This approach divides the problem size by half at each step, leading to a more efficient solution.

#### Code

```java
public class Solution {
    public double myPow(double x, int n) {
        if (n == 0) return 1;
        if (n < 0) {
            x = 1 / x;
            n = -n;
        }
        return powRecursive(x, n);
    }
    
    private double powRecursive(double x, int n) {
        if (n == 0) return 1;
        double half = powRecursive(x, n / 2);
        // Check if n is even or odd
        if (n % 2 == 0) {
            return half * half;
        } else {
            return x * half * half;
        }
    }
}
```

#### Time Complexity

- **O(log n)**: Recursive function halves `n` at each call.

#### Space Complexity

- **O(log n)**: Call stack space for recursion.

---

### Iterative Binary Exponentiation (Optimal)

#### Intuition

The iterative method of binary exponentiation is similar to the recursive method, but it avoids the overhead of recursion. We use bit manipulation to quickly determine if the current power `n` is odd and needs additional multiplication by `x`. This results in efficient exponentiation with reduced time complexity.

#### Code

```java
public class Solution {
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double result = 1.0;
        double currentProduct = x;

        // Iterate while there's still power to consider
        while (N > 0) {
            // If N is odd, multiply result by current product
            if (N % 2 == 1) {
                result *= currentProduct;
            }
            currentProduct *= currentProduct; // Square the product for next power
            N /= 2; // Divide N by 2
        }

        return result;
    }
}
```

#### Time Complexity

- **O(log n)**: The powering operation uses logarithmic number of multiplications.

#### Space Complexity

- **O(1)**: This approach does not use additional space.

This iterative binary exponentiation approach is the most efficient and optimal solution in terms of both time and space, making it suitable for large values of `n`.

