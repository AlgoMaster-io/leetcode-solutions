# 357. [Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits/)

## Approach 1: Recursive Backtracking

### Solution
typescript
```typescript
// Time Complexity: O(10^n)
// Space Complexity: O(n)
class Solution {
    countNumbersWithUniqueDigits(n: number): number {
        if (n === 0) return 1;
        const used: boolean[] = new Array(10).fill(false);
        return this.countNumbers(0, n, used);
    }

    private countNumbers(current: number, n: number, used: boolean[]): number {
        if (n === 0) return 1;
        let count = 1; // count the number itself
        for (let i = 0; i < 10; i++) {
            if (!used[i]) {
                used[i] = true;
                count += this.countNumbers(current * 10 + i, n - 1, used);
                used[i] = false;
            }
        }
        return count;
    }
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    countNumbersWithUniqueDigits(n: number): number {
        if (n === 0) return 1;

        let uniqueNumbers = 10, availableDigits = 9, currentUnique = 9;

        // Use dynamic programming to calculate the number
        for (let i = 2; i <= n; i++) {
            currentUnique *= availableDigits;
            uniqueNumbers += currentUnique;
            availableDigits--;
        }

        return uniqueNumbers;
    }
}
```

## Approach 3: Formula-based Calculation

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
    countNumbersWithUniqueDigits(n: number): number {
        if (n === 0) return 1;

        let count = 10;  // start counting from single-digit numbers
        let uniqueDigits = 9, availableNumbers = 9;

        // Calculate using the permutation formula for unique digits
        while (n > 1 && availableNumbers > 0) {
            uniqueDigits *= availableNumbers;
            count += uniqueDigits;
            availableNumbers--;
            n--;
        }

        return count;
    }
}
```

