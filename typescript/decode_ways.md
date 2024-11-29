# 91. [Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approach 1: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function numDecodings(s: string): number {
    return decodeHelper(s, 0, new Map<number, number>());
}

function decodeHelper(s: string, index: number, memo: Map<number, number>): number {
    // If we've reached the end of the string, return 1 for a valid path
    if (index === s.length) {
        return 1;
    }
    
    // If the current portion of the string starts with '0', it's invalid
    if (s.charAt(index) === '0') {
        return 0;
    }
    
    // If we've already computed the result for this index, return it
    if (memo.has(index)) {
        return memo.get(index)!;
    }
    
    // Decode one character
    let result = decodeHelper(s, index + 1, memo);
    
    // Decode two characters if it's a valid two-digit number
    if (index + 1 < s.length && 
        (s.charAt(index) === '1' || 
        (s.charAt(index) === '2' && s.charAt(index + 1) < '7'))) {
        result += decodeHelper(s, index + 2, memo);
    }
    
    // Save the result to the memoization map
    memo.set(index, result);
    
    return result;
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function numDecodings(s: string): number {
    if (s === null || s.length === 0 || s.charAt(0) === '0') return 0;

    const n = s.length;
    const dp: number[] = Array(n + 1).fill(0);
    
    dp[0] = 1;  // Base case: an empty string has one way to decode
    dp[1] = 1;  // Base case: the first character can't be '0'
    
    for (let i = 2; i <= n; i++) {
        const oneDigit = parseInt(s.substring(i - 1, i));
        const twoDigits = parseInt(s.substring(i - 2, i));
        
        if (oneDigit >= 1 && oneDigit <= 9) {
            dp[i] += dp[i - 1];
        }
        
        if (twoDigits >= 10 && twoDigits <= 26) {
            dp[i] += dp[i - 2];
        }
    }
    
    return dp[n];
}
```

## Approach 3: Dynamic Programming with Space Optimization

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
function numDecodings(s: string): number {
    if (s === null || s.length === 0 || s.charAt(0) === '0') return 0;

    const n = s.length;
    let prev2 = 1;  // dp[i-2]
    let prev1 = 1;  // dp[i-1]
    
    for (let i = 1; i < n; i++) {
        let current = 0;
        const oneDigit = parseInt(s.substring(i, i + 1));
        const twoDigits = parseInt(s.substring(i - 1, i + 1));
        
        if (oneDigit >= 1 && oneDigit <= 9) {
            current += prev1;
        }
        
        if (twoDigits >= 10 && twoDigits <= 26) {
            current += prev2;
        }
        
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}
```

