
# 233. [Number of Digit One](https://leetcode.com/problems/number-of-digit-one/)

## Approach 1: Brute Force

### Solution
typescript
```typescript
// Time Complexity: O(n * log10(n))
// Space Complexity: O(1)
class Solution {
    countDigitOne(n: number): number {
        let count = 0;

        // Loop through each number from 1 to n
        for (let i = 1; i <= n; i++) {
            count += this.countOnesInNumber(i);
        }

        return count;
    }

    // Helper method to count number of 1's in a single number
    private countOnesInNumber(num: number): number {
        let ones = 0;
        while (num > 0) {
            if (num % 10 === 1) {
                ones++;
            }
            num = Math.floor(num / 10);
        }
        return ones;
    }
}
```

## Approach 2: Counting 1s Using Digit Position

### Solution
typescript
```typescript
// Time Complexity: O(log10(n))
// Space Complexity: O(1)
class Solution {
    countDigitOne(n: number): number {
        let count = 0;
        let factor = 1;  // Factor to isolate digits, start with ones place

        while (factor <= n) {
            const lowerNumbers = n - Math.floor(n / factor) * factor; // Numbers less than current digit place
            const currentDigit = Math.floor(n / factor) % 10; // Current digit

            const higherNumbers = Math.floor(n / (factor * 10)); // Numbers higher than current digit place

            if (currentDigit === 0) {
                count += higherNumbers * factor; // When the current digit is '0'
            } else if (currentDigit === 1) {
                count += higherNumbers * factor + lowerNumbers + 1; // When the current digit is '1'
            } else {
                count += (higherNumbers + 1) * factor; // When the current digit is '2' or more
            }

            factor *= 10; // Move to next digit position
        }

        return count;
    }
}
```

