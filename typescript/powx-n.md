# [Leetcode 50: Pow(x, n)](https://leetcode.com/problems/powx-n/)

## Approaches
1. [Brute Force Iterative Approach](#brute-force-iterative-approach)
2. [Recursive Approach](#recursive-approach)
3. [Iterative Binary Exponentiation](#iterative-binary-exponentiation)

---

### Brute Force Iterative Approach

#### Intuition
The simplest idea would be to multiply the number `x` by itself `n` times. This approach works directly but only for small values of `n`, as it runs in linear time.

#### Implementation

```typescript
function myPow(x: number, n: number): number {
    if (n === 0) return 1; // When n is 0, x^0 is always 1
    if (n < 0) {
        x = 1 / x; // If n is negative, convert to positive and invert x
        n = -n;
    }
    let result = 1;
    for (let i = 0; i < n; i++) {
        result *= x; // Multiply x, n times
    }
    return result;
}
```

#### Time Complexity
- O(n): We multiply `x` by itself `n` times.

#### Space Complexity
- O(1): Only constant space used for variables.

---

### Recursive Approach

#### Intuition
We can leverage recursion by dividing the power into halves, reducing the problem size by half at each step. Here, `x^n` can be calculated by `x^(n/2) * x^(n/2)` if `n` is even, and `x * x^((n-1)/2) * x^((n-1)/2)` if `n` is odd, thus reducing redundant calculations.

#### Implementation

```typescript
function myPow(x: number, n: number): number {
    if (n === 0) return 1;
    if (n < 0) {
        x = 1 / x;
        n = -n;
    }

    return powRecursive(x, n);

    function powRecursive(x: number, n: number): number {
        if (n === 0) return 1;
        const half = powRecursive(x, Math.floor(n / 2));
        if (n % 2 == 0) {
            return half * half; // n is even
        } else {
            return half * half * x; // n is odd
        }
    }
}
```

#### Time Complexity
- O(log n): We reduce `n` by half each time.

#### Space Complexity
- O(log n): Recursive call stack depth.

---

### Iterative Binary Exponentiation

#### Intuition
By using the iterative binary exponentiation technique, we keep multiplying the base `x` with the result only when the current bit of the exponent is 1. We keep squaring `x` while reducing `n` by half. This is both efficient and avoids recursion.

#### Implementation

```typescript
function myPow(x: number, n: number): number {
    if (n === 0) return 1;
    if (n < 0) {
        x = 1 / x;
        n = -n;
    }

    let result = 1;
    while (n > 0) {
        if (n % 2 === 1) {
            result *= x; // If current least significant bit is 1, multiply by x
        }
        x *= x; // Square the base
        n = Math.floor(n / 2); // Shift bits to right
    }
    return result;
}
```

#### Time Complexity
- O(log n): We process each bit of `n`.

#### Space Complexity
- O(1): No extra space besides variables.

