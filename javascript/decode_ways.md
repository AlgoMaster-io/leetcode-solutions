# 91. [Decode Ways](https://leetcode.com/problems/decode-ways/)

## Approach 1: Recursion with Memoization

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function numDecodings(s) {
    return decodeHelper(s, 0, {});
}

function decodeHelper(s, index, memo) {
    // If we've reached the end of the string, return 1 for a valid path
    if (index === s.length) {
        return 1;
    }

    // If the current portion of the string starts with '0', it's invalid
    if (s[index] === '0') {
        return 0;
    }

    // If we've already computed the result for this index, return it
    if (memo.hasOwnProperty(index)) {
        return memo[index];
    }

    // Decode one character
    let result = decodeHelper(s, index + 1, memo);

    // Decode two characters if it's a valid two-digit number
    if (index + 1 < s.length &&
        (s[index] === '1' || 
        (s[index] === '2' && s[index + 1] < '7'))) {
        result += decodeHelper(s, index + 2, memo);
    }

    // Save the result to the memoization map
    memo[index] = result;

    return result;
}
```

## Approach 2: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
function numDecodings(s) {
    if (s == null || s.length === 0 || s[0] === '0') return 0;
    
    const n = s.length;
    const dp = new Array(n + 1).fill(0);
    
    dp[0] = 1;  // Base case: an empty string has one way to decode
    dp[1] = 1;  // Base case: the first character can't be '0'
    
    for (let i = 2; i <= n; i++) {
        const oneDigit = parseInt(s.substring(i - 1, i), 10);
        const twoDigits = parseInt(s.substring(i - 2, i), 10);
        
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
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)
function numDecodings(s) {
    if (s == null || s.length === 0 || s[0] === '0') return 0;

    const n = s.length;
    let prev2 = 1;  // dp[i-2]
    let prev1 = 1;  // dp[i-1]
    
    for (let i = 1; i < n; i++) {
        let current = 0;
        const oneDigit = parseInt(s.substring(i, i + 1), 10);
        const twoDigits = parseInt(s.substring(i - 1, i + 1), 10);
        
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

