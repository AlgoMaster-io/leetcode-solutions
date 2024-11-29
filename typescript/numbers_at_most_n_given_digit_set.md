# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
typescript
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D

function atMostNGivenDigitSet(digits: string[], n: number): number {
    const nStr = n.toString();
    const len = nStr.length;
    
    // Convert digit strings to character array
    const digitArr = digits.map(d => d.charAt(0));

    return backtrack(digitArr, nStr, 0, true);
}

/**
 * Recursive function to calculate count of numbers
 */
function backtrack(digits: string[], nStr: string, index: number, limit: boolean): number {
    if (index === nStr.length) { // Base case: reached the length of nStr
        return 1;
    }

    let count = 0;
    for (const d of digits) {
        if (limit && d > nStr.charAt(index)) {
            break; // No need to consider digits greater than current nStr digit
        }
        count += backtrack(digits, nStr, index + 1, limit && d === nStr.charAt(index));
    }

    // Add numbers with length less than nStr (full set combinations)
    if (!limit) {
        let factor = 1;
        for (let i = index + 1; i < nStr.length; i++) {
            factor *= digits.length; // for each position, we have `|digits|` choices
        }
        count += factor;
    }

    return count;
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D

function atMostNGivenDigitSet(digits: string[], n: number): number {
    const nStr = n.toString();
    const len = nStr.length;
    const dp = new Array(len + 1).fill(0);
    dp[len] = 1;

    // Convert digit strings to character array
    const digitArr = digits.map(d => d.charAt(0));

    for (let i = len - 1; i >= 0; i--) {
        const c = nStr.charAt(i);
        for (const d of digitArr) {
            if (d < c) {
                dp[i] += Math.pow(digits.length, len - i - 1);
            } else if (d === c) {
                dp[i] += dp[i + 1];
            }
        }
    }

    let result = 0;
    for (let i = 1; i < len; i++) {
        result += Math.pow(digits.length, i);
    }
    return result + dp[0];
}
```

## Approach 3: Mathematical Counting

### Solution
typescript
```typescript
// Time Complexity: O(n * m)
// Space Complexity: O(1)
// where n is the number of digits in N and m is the number of digits in D

function atMostNGivenDigitSet(digits: string[], n: number): number {
    const nStr = n.toString();
    const len = nStr.length;
    const count: [number, number][] = Array.from({ length: len }, () => [0, 0]);

    // Precompute the power array to avoid repetitive calculation
    const power = new Array(len + 1).fill(1);
    for (let i = 1; i <= len; i++) {
        power[i] = power[i - 1] * digits.length;
    }

    for (let i = 0; i < len; i++) {
        const c = nStr.charAt(i);
        for (const digit of digits) {
            const d = digit.charAt(0);

            if (d < c) {
                count[i][0] += power[len - i - 1];
            } else if (d === c) {
                count[i][1] = 1;
            }
        }
        if (count[i][1] === 0) break;
    }

    let result = 0;
    for (let i = 0; i < len; i++) {
        result += count[i][0];
    }

    for (let i = 1; i < len; i++) {
        result += power[i];
    }

    result += count[len - 1][1];

    return result;
}
```

