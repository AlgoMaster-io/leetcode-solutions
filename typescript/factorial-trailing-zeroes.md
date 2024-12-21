[Leetcode Problem: Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

## Table of Contents
1. [Approach 1: Iterative Approach](#approach-1)
2. [Approach 2: Optimized Mathematical Approach](#approach-2)

---

### Approach 1: Iterative Approach

The trailing zeroes in any number are the result of multiplying factors of 10. Thus, to compute the number of trailing zeroes in the factorial of a number `n`, we need to determine how many times 10 is a factor in the numbers from 1 to `n`.

Since 10 is the product of 2 and 5, in the factorial, there will always be more factors of 2 than factors of 5. Thus, the number of trailing zeroes is determined by the number of times 5 is a factor in the multiples of 5 up to `n`.

**Intuition:**

- Count all multiples of 5 up to `n`, each contributes at least one factor of 5.
- Count multiples of 25, 125, etc., because these numbers have more factors of 5.

```typescript
function trailingZeroes(n: number): number {
    let count = 0;
    while (n >= 5) {
        // Counting how many multiples of 5, 25, 125... are present
        n = Math.floor(n / 5);
        count += n; 
    }
    return count;
}
```

**Time Complexity:** O(log5(n)) - We are dividing `n` by 5 repeatedly.
**Space Complexity:** O(1) - We're using a constant amount of space for variable `count`.

---

### Approach 2: Optimized Mathematical Approach

The solution explained in Approach 1 is already quite optimal, as we're iterating over log5(n) divisions. However, let's focus more on understanding the mathematics behind why we only care about the number of 5s.

**Intuition:**

- Whenever we perform integer division by `5`, we are effectively counting the number of multiples of `5` in the range.
- The additional multiples (like 25, 125) contribute extra counts of 5 - because `25` = 5^2, `125`=5^3, which means each contributes extra 5's.
- The idea is to just keep dividing the number by `5` and accumulate the count of multiples.

The iterative nature here is the most effective approach for this mathematical operation with factorials, and we've already distilled this idea into a minimal form.

The code remains the same as in the iterative method because it is already optimal.

```typescript
function trailingZeroes(n: number): number {
    let count = 0;
    while (n >= 5) {
        n = Math.floor(n / 5);
        count += n; 
    }
    return count;
}
```

**Time Complexity:** O(log5(n)) - We are dividing `n` by 5 repeatedly, as explained.
**Space Complexity:** O(1) - Using constant space.

---

The solution using integer division is both intuitive and optimal for the problem of finding trailing zeroes in factorial computations. The key insight is the significance of factors of 5 in creating trailing zeroes in products of sequential numbers.

