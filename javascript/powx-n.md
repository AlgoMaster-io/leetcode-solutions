# [Leetcode 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approaches
1. [Brute Force Approach](#brute-force-approach)
2. [Recursive Approach (Divide and Conquer)](#recursive-approach-divide-and-conquer)
3. [Iterative Approach (Exponentiation by Squaring)](#iterative-approach-exponentiation-by-squaring)

---

### Brute Force Approach

#### Intuition
The most straightforward method is to multiply `x` by itself `n` times. This is an intuitive approach but not efficient for large `n` due to its linear time complexity.

```javascript
function myPow(x, n) {
    // Handle special case when n is 0, any number to power 0 is 1
    if (n === 0) return 1;

    // Calculate the absolute value of n
    let N = Math.abs(n);

    // Initialize result
    let result = 1;

    // Multiply x to itself N times
    for (let i = 0; i < N; i++) {
        result *= x;
    }

    // If n is negative, return the inverse
    return n < 0 ? 1 / result : result;
}
```

**Time Complexity**: O(n) — We iterate `n` times, performing a single multiplication each time.

**Space Complexity**: O(1) — We only use a constant amount of extra space.

---

### Recursive Approach (Divide and Conquer)

#### Intuition
We can improve efficiency by reducing the number of multiplications needed. The idea is based on the property: 
- `x^n = (x^(n/2))^2` if `n` is even.
- `x^n = x * x^(n-1)` if `n` is odd.

We use recursion to break down the problem, effectively using a divide-and-conquer strategy.

```javascript
function myPow(x, n) {
    // Helper function for recursion
    function pow(x, n) {
        // Base case for recursion
        if (n === 0) return 1;

        let half = pow(x, Math.floor(n / 2));

        // If n is even
        if (n % 2 === 0) {
            return half * half;
        } 
        // If n is odd
        else {
            return half * half * x;
        }
    }

    // Handle negative powers by calling pow with absolute value and taking the reciprocal
    let result = pow(x, Math.abs(n));
    return n < 0 ? 1 / result : result;
}
```

**Time Complexity**: O(log n) — Each recursive call reduces `n` by half, leading to a logarithmic number of multiplications.

**Space Complexity**: O(log n) — The stack depth due to recursion is proportional to `log n`.

---

### Iterative Approach (Exponentiation by Squaring)

#### Intuition
To further optimize and avoid the extra recursion stack overhead, we can use the iterative approach, which implements the same logic as the recursive method but in an iterative manner.

```javascript
function myPow(x, n) {
    // Handle special case when n is 0
    if (n === 0) return 1;

    let N = Math.abs(n);
    let result = 1;
    let currentProduct = x;

    // Loop until n is 0
    while (N > 0) {
        // If N is odd, multiply the result by current product
        if (N % 2 === 1) {
            result *= currentProduct;
        }
        // Square the current product for next bit
        currentProduct *= currentProduct;
        // Shift N down by 1 bit
        N = Math.floor(N / 2);
    }

    // If original power was negative, invert the result
    return n < 0 ? 1 / result : result;
}
```

**Time Complexity**: O(log n) — Similar to the recursive approach, the number of operations is logarithmic due to the halving of `N`.

**Space Complexity**: O(1) — We use only a constant amount of extra space. 

---

Each of these solutions demonstrates a different approach to solving the problem with varying efficiency and clarity. The iterative approach is generally preferred for its balance of efficiency and simplicity without recursion overhead.

