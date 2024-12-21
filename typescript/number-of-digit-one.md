# [Leetcode 233: Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Mathematical Counting Digits](#mathematical-counting-digits)
- [Optimized Counting with Place Values](#optimized-counting-with-place-values)

---

### Brute Force Approach

#### Intuition

The simplest approach to this problem is to iterate over every number from 1 to `n` and count the number of '1's in each of those numbers. Although straightforward, this approach can be inefficient for large values of `n` because we must inspect each number individually.

#### Code

```typescript
function countDigitOneBrute(n: number): number {
    let count = 0;
    for (let i = 1; i <= n; i++) {
        count += countOnesInNumber(i);
    }
    return count;
}

function countOnesInNumber(num: number): number {
    let onesCount = 0;
    while (num > 0) {
        if (num % 10 === 1) {
            onesCount += 1;
        }
        num = Math.floor(num / 10);
    }
    return onesCount;
}
```

#### Time Complexity

- **Time Complexity**: O(n * log10(n)), where `n` is the given number. We process each number up to n, and each number consists of log10(n) digits in the worst case.
- **Space Complexity**: O(1), as we are only using a constant amount of extra space.

---

### Mathematical Counting Digits

#### Intuition

Instead of iterating over every single number, we can derive a formula or pattern for the number of ones based on the observations of groups of digits. By systematically analyzing the positions of digits, we can count the occurrences of '1' without having to explicitly iterate over each number.

#### Code

```typescript
function countDigitOneMath(n: number): number {
    let count = 0;
    
    // Traverse over each digit
    for (let m = 1; m <= n; m *= 10) {
        const a = Math.floor(n / m);
        const b = n % m;
        
        // Calculate count of '1's contributed by the current digit
        count += Math.floor((a + 8) / 10) * m + (a % 10 === 1 ? b + 1 : 0);
    }
    
    return count;
}
```

#### Time Complexity

- **Time Complexity**: O(log10(n)), because we compute based on the number of digits in `n`.
- **Space Complexity**: O(1), as no additional space is required.

---

### Optimized Counting with Place Values

#### Intuition

We further optimize by breaking it down into contributions from each digit while considering the place value and digit itself, focusing on digits at each place from least significant to most with conditions:

- How many full sets we have seen at current place.
- Streamlined formula to figure out custom conditions where '1' contributes differently.

The core idea is using `place * ((n // place + 8) // 10)` to derive sets and additions based on if `currentDigit === 1`.

#### Code

```typescript
function countDigitOneOptimized(n: number): number {
    let count = 0;
    let place = 1;
    
    while (place <= n) {
        const current = Math.floor((n / place) % 10);
        const before = Math.floor(n / (place * 10));
        const after = n % place;
        
        // Each segment contributes to the count of '1' in a pattern-based manner.
        count += before * place;
        
        if (current === 1) {
            count += after + 1;
        } else if (current > 1) {
            count += place;
        }
        
        place *= 10;
    }
    
    return count;
}
```

#### Time Complexity

- **Time Complexity**: O(log10(n)), since the operation depends on the number of digits in `n`.
- **Space Complexity**: O(1), requires a constant amount of space regardless of input size.

---

These approaches highlight how understanding and using mathematical properties of numbers can greatly optimize what might initially seem like a brute force problem.

