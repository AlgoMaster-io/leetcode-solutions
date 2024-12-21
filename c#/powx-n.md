# [Leetcode 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Solutions Overview
- [Approach 1: Iterative Multiplication](#approach-1-iterative-multiplication)
- [Approach 2: Fast Exponentiation (Recursive)](#approach-2-fast-exponentiation-recursive)
- [Approach 3: Fast Exponentiation (Iterative)](#approach-3-fast-exponentiation-iterative)

## Approach 1: Iterative Multiplication

### Intuition
A straightforward way to calculate `x^n` is to multiply `x` by itself `n` times. While this is very intuitive, it is inefficient for large values of `n`, as we directly perform `n` multiplications.

### Implementation

```csharp
public class Solution {
    public double MyPow(double x, int n) {
        // Handle edge case where n is zero, any number to the power of 0 is 1
        if (n == 0) return 1;

        // Convert n to a long to handle edge cases where n = int.MinValue
        long longN = n;
        // If n is negative, we work with its positive equivalent and invert the result
        if (longN < 0) {
            longN = -longN;
            x = 1 / x;
        }

        double result = 1;
        // Multiply x to result longN times
        while (longN > 0) {
            result *= x;
            longN--;
        }
        return result;
    }
}
```

### Time Complexity
- **Time:** O(n), where `n` is the exponent.
- **Space:** O(1), as we use only a few variables for calculation and the space requirement doesn't grow with input size.

## Approach 2: Fast Exponentiation (Recursive)

### Intuition
Fast exponentiation, also known as exponentiation by squaring, reduces the time complexity by half at each recursive call. If `n` is even, `x^n = (x^2)^(n/2)`. If `n` is odd, `x^n = x * (x^2)^(n//2)`. This dramatically reduces the number of multiplications needed from linear to logarithmic.

### Implementation

```csharp
public class Solution {
    public double MyPow(double x, int n) {
        // Convert n to a long to handle edge cases where n = int.MinValue
        long longN = n;
        // If n is negative, we work with its positive equivalent and invert the result
        if (longN < 0) {
            longN = -longN;
            x = 1 / x;
        }

        return fastPow(x, longN);
    }
    
    // Helper method for computing power recursively
    private double fastPow(double x, long n) {
        if (n == 0) return 1; // Base case: any number to the power of 0 is 1
        // Recursively calculate half power
        double half = fastPow(x, n / 2);
        if (n % 2 == 0) {
            // If even, just square the half
            return half * half;
        } else {
            // If odd, multiply by an additional "x"
            return half * half * x;
        }
    }
}
```

### Time Complexity
- **Time:** O(log n), because we reduce `n` by approximately half at each step.
- **Space:** O(log n), due to the recursion stack from recursive calls.

## Approach 3: Fast Exponentiation (Iterative)

### Intuition
We can achieve the fast exponentiation iteratively using bit manipulation. We keep track of the current power of `x` and multiply it into the result only if the current bit of `n` is set. The iterative solution can avoid the recursive stack.

### Implementation

```csharp
public class Solution {
    public double MyPow(double x, int n) {
        long longN = n;
        if (longN < 0) {
            longN = -longN;
            x = 1 / x;
        }
        
        double result = 1.0;
        double currentProduct = x;
        while (longN > 0) {
            // If the least significant bit is 1, multiply the current product
            if ((longN % 2) == 1) {
                result *= currentProduct;
            }
            // Square the current product for the next bit
            currentProduct *= currentProduct;
            // Shift right by one bit (equivalent to dividing by 2)
            longN /= 2;
        }
        return result;
    }
}
```

### Time Complexity
- **Time:** O(log n), for the same reasons as the recursive version: reducing the exponent by half each iteration.
- **Space:** O(1), as we managed to avoid the recursive call stack. 

This iterative solution is often preferred for its constant space usage.

