# [Leetcode Problem 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approaches:

1. [Recursive Approach](#recursive-approach)
2. [Iterative Approach using Fast Exponentiation](#iterative-approach-using-fast-exponentiation)

---

### Recursive Approach

The basic idea is to utilize the mathematical property of exponents: 
\[ x^n = x^{n/2} \times x^{n/2} \text{ if } n \text{ is even} \]  
\[ x^n = x \times x^{n-1} \text{ if } n \text{ is odd} \]  

#### Intuition:

The goal is to reduce the number of multiplications needed to compute \( x^n \). By using recursion, we can break down the problem into smaller subproblems. 

- If \( n = 0 \), \( x^n \) is \( 1 \) (since any number raised to the power \( 0 \) is \( 1 \)).
- If \( n < 0 \), we need to calculate \( \frac{1}{x^n} \).
- For positive \( n \), we recursively calculate \( x^{n/2} \) and then square the result if \( n \) is even, or multiply it by \( x \) if \( n \) is odd.

#### Code:

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if (n == 0)
            return 1.0;  // Base case: x^0 = 1
        if (n < 0) { 
            x = 1 / x;  // For negative exponents, take reciprocal
            n = -n;
        }
        return helperPow(x, n);
    }
    
    double helperPow(double x, int n) {
        if (n == 0)
            return 1.0;
        double half = helperPow(x, n / 2);
        if (n % 2 == 0) {
            return half * half;  // n is even
        } else {
            return half * half * x;  // n is odd
        }
    }
};
```

#### Complexity:

- **Time Complexity**: \( O(\log n) \) because we are dividing the problem into half at each step.
- **Space Complexity**: \( O(\log n) \) due to the recursion stack.

---

### Iterative Approach using Fast Exponentiation

In this approach, instead of using recursion, we can use an iterative method to achieve the same effect.

#### Intuition:

We maintain a result that starts at 1, and we repeatedly square the base and halve the exponent, suppressing the need for recursion. If at any step the exponent is odd, we multiply the result by the current base.

#### Code:

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        long long N = n;  // Handle negatives correctly in c++ where abs(INT_MIN) might overflow
        if (N < 0) {
            x = 1 / x;  // For negative exponents, take reciprocal
            N = -N;
        }
        double res = 1.0;
        while (N) {
            if (N % 2 == 1) {
                res *= x;  // If N is odd, multiply the result by x
            }
            x *= x;  // Square the base
            N /= 2;  // Halve the exponent
        }
        return res;
    }
};
```

#### Complexity:

- **Time Complexity**: \( O(\log n) \) because we are halving the exponent at each step.
- **Space Complexity**: \( O(1) \), as we use constant extra space. 

This iterative approach is more space-efficient than the recursive one, as it avoids the call stack overhead.

