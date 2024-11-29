# 902. [Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approach 1: Simple Counting with Recursive Backtracking

### Solution
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
class Solution {
    atMostNGivenDigitSet(digits, n) {
        const nStr = n.toString();
        
        return this.backtrack(digits, nStr, 0, true);
    }
    
    // Recursive function to calculate count of numbers
    backtrack(digits, nStr, index, limit) {
        if (index === nStr.length) {
            return 1;
        }
        
        let count = 0;
        for (const d of digits) {
            if (limit && d > nStr[index]) {
                break; // No need to consider digits greater than current nStr digit
            }
            count += this.backtrack(digits, nStr, index + 1, limit && d === nStr[index]);
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
}
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(n)
// where n is the number of digits in N and m is the number of digits in D
class Solution {
    atMostNGivenDigitSet(digits, n) {
        const nStr = n.toString();
        const len = nStr.length;
        const dp = new Array(len + 1).fill(0);
        dp[len] = 1;

        for (let i = len - 1; i >= 0; i--) {
            const c = nStr[i];
            for (const d of digits) {
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
}
```

## Approach 3: Mathematical Counting

### Solution
```javascript
// Time Complexity: O(n * m)
// Space Complexity: O(1)
// where n is the number of digits in N and m is the number of digits in D
class Solution {
    atMostNGivenDigitSet(digits, n) {
        const nStr = n.toString();
        const len = nStr.length;
        const count = Array.from({length: len}, () => [0, 0]);

        // Precompute the power array to avoid repetitive calculation
        const power = new Array(len + 1);
        power[0] = 1;
        for (let i = 1; i <= len; i++) {
            power[i] = power[i - 1] * digits.length;
        }

        for (let i = 0; i < len; i++) {
            const c = nStr[i];
            for (const digit of digits) {
                const d = digit;
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
}
```

