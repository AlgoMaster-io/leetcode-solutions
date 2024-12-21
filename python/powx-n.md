# [Leetcode 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Recursive Approach](#optimized-recursive-approach)
3. [Iterative Exponentiation by Squaring](#iterative-exponentiation-by-squaring)

### Brute Force Approach

#### Intuition
The most straightforward approach is to multiply `x` by itself `n` times, i.e., computing \( x^n \) using a simple loop. This method directly follows the mathematical definition of power but is inefficient for large `n` as it requires `n` multiplications.

#### Implementation
```python
def myPow(x: float, n: int) -> float:
    # Handle the case where n is zero
    if n == 0:
        return 1
    
    # Initialize result
    result = 1
    
    # Handle negative powers by using the reciprocal of x
    exponent = abs(n)
    
    for _ in range(exponent):
        result *= x
    
    # If n is negative, return the reciprocal
    return result if n > 0 else 1 / result
```

#### Time Complexity
- **Time:** O(n) - We iterate `n` times.
- **Space:** O(1) - Constant space usage.

### Optimized Recursive Approach

#### Intuition
This method uses the concept of divide and conquer. We can reduce the number of multiplications by observing the property:
\[ x^n = (x^{n/2}) \cdot (x^{n/2}) \]
If `n` is even, and
\[ x^n = x \cdot (x^{(n-1)/2}) \cdot (x^{(n-1)/2}) \]
If `n` is odd. This approach reduces the number of recursive calls essentially by dividing `n` by half in most cases, speeding up the calculation.

#### Implementation
```python
def myPow(x: float, n: int) -> float:
    # Helper function to perform fast exponentiation
    def fastPow(y, m):
        if m == 0:
            return 1
        half = fastPow(y, m // 2)
        if m % 2 == 0:
            return half * half
        else:
            return half * half * y
    
    # Handle negative power by adjusting x and using positive power
    if n < 0:
        x = 1 / x
        n = -n
        
    return fastPow(x, n)
```

#### Time Complexity
- **Time:** O(log n) - Each step reduces the problem into half.
- **Space:** O(log n) - Recursive stack space.

### Iterative Exponentiation by Squaring

#### Intuition
This method follows the same principle as the optimized recursive approach but avoids the stack overhead by converting the recursion into iteration. We continuously square the base and adjust the result when the power is odd until the power is zero.

#### Implementation
```python
def myPow(x: float, n: int) -> float:
    # Handle negative powers by using the reciprocal of x
    if n < 0:
        x = 1 / x
        n = -n
    
    # Initialize result
    result = 1
    
    while n > 0:
        # If n is odd, multiply x with the result
        if n % 2 == 1:
            result *= x
        
        # Square the base
        x *= x
        
        # Halve the power
        n //= 2
    
    return result
```

#### Time Complexity
- **Time:** O(log n) - Each iteration reduces `n` by half.
- **Space:** O(1) - Constant space usage, no recursive stack.

These approaches provide several ways to tackle the power function, each improving upon the previous by leveraging mathematical insights for efficiency.

