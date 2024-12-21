# [LeetCode 902: Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/)

## Approaches
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

## Approach 1: Brute Force

### Intuition
The problem is asking us to find numbers up to a maximum number \( N \) using a given set of digits. A brute force way is to generate all the possible numbers using these digits and count those which are lesser than or equal to \( N \).

### Algorithm
1. Convert the number \( N \) to a string, so we can compare digit by digit.
2. Calculate the number of digits in \( N \).
3. For each length less than the digit length of \( N \), count all possible combinations.
4. For numbers with the same length as \( N \), handle digit-by-digit comparison to ensure we don't generate numbers greater than \( N \).

### Code
```typescript
function atMostNGivenDigitSet(digits: string[], N: number): number {
    const S = String(N);
    const K = S.length;
    let result = 0;

    // Calculate numbers with fewer digits than N
    for (let i = 1; i < K; i++) {
        result += Math.pow(digits.length, i);
    }

    // Function to pick numbers with exactly K digits
    // This will make sure to count numbers that are less than or equal digit by digit
    const pickNumbers = (index: number): boolean => {
        if (index === K) {
            return true;
        }
        
        const s = S[index];
        
        for (const d of digits) {
            if (d < s) {
                result += Math.pow(digits.length, K - index - 1);
            } else if (d === s) {
                if (pickNumbers(index + 1)) {
                    return true;
                }
            }
        }
        
        return false;
    };

    // Start processing each digit of N
    pickNumbers(0);
    
    return result;
}
```

### Time Complexity
Since we generate all possible combinations, the time complexity is approximately \( O(K \cdot D^K) \) where \( D \) is the number of available digits, and \( K \) is the number of digits in \( N \).

### Space Complexity
The space complexity is \( O(K) \) due to the recursion stack.

## Approach 2: Dynamic Programming

### Intuition
Instead of checking number by number, we can use dynamic programming to store the number of valid combinations up to each position. This is efficient as it prevents recalculating combinations repeatedly.

### Algorithm
1. Calculate and store the number of valid numbers fewer than \( N \) using available digits.
2. Use a DP array where each entry represents the count of valid numbers from that starting digit.
3. For each digit available in the set, calculate how many numbers can be formed using that as their leading digit and fill the DP table accordingly.

### Code
```typescript
function atMostNGivenDigitSet(digits: string[], N: number): number {
    const S = String(N);
    const K = S.length;
    const dp: number[] = Array(K + 1).fill(0);
    dp[K] = 1;

    for (let i = K - 1; i >= 0; i--) {
        const s = S[i];
        for (const d of digits) {
            if (d < s) {
                dp[i] += Math.pow(digits.length, K - i - 1);
            } else if (d === s) {
                dp[i] += dp[i + 1];
            }
        }
    }

    // Sum up all possible numbers with fewer digits than K
    for (let i = 1; i < K; i++) {
        dp[0] += Math.pow(digits.length, i);
    }

    return dp[0];
}
```

### Time Complexity
The time complexity is \( O(K \cdot D) \) where \( D \) is the number of digits in the set, and \( K \) is the number of digits in \( N \).

### Space Complexity
The space complexity is \( O(K) \) due to the DP array.

