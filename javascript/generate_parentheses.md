# 22. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

## Approach 1: Brute Force (Generate All Combinations)

### Solution
```javascript
// Time Complexity: O(2^(2n) * n), where 2^(2n) is the number of combinations and n is the time to validate each combination
// Space Complexity: O(2^(2n) * n) for storing the combinations

var generateParenthesis = function(n) {
    const result = [];
    generateAll(new Array(2 * n), 0, result);
    return result;
};

var generateAll = function(current, pos, result) {
    if (pos === current.length) {
        if (isValid(current)) {
            result.push(current.join('')); // Add valid combination to result
        }
        return;
    }

    current[pos] = '('; // Place an open parenthesis
    generateAll(current, pos + 1, result);
    current[pos] = ')'; // Place a close parenthesis
    generateAll(current, pos + 1, result);
};

var isValid = function(current) {
    let balance = 0;
    for (let c of current) {
        if (c === '(') {
            balance++;
        } else {
            balance--;
        }
        if (balance < 0) {
            return false; // Invalid if closing parenthesis exceeds opening
        }
    }
    return balance === 0; // Valid if balance is zero
};
```

## Approach 2: Backtracking (Optimal Solution)

### Solution
```javascript
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations

var generateParenthesis = function(n) {
    const result = [];
    backtrack(result, [], 0, 0, n);
    return result;
};

var backtrack = function(result, current, open, close, max) {
    if (current.length === max * 2) {
        result.push(current.join('')); // Add valid combination to result
        return;
    }

    if (open < max) {
        current.push('('); // Add an open parenthesis
        backtrack(result, current, open + 1, close, max);
        current.pop(); // Backtrack
    }
    if (close < open) {
        current.push(')'); // Add a close parenthesis
        backtrack(result, current, open, close + 1, max);
        current.pop(); // Backtrack
    }
};
```

## Approach 3: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(4^n / sqrt(n)), Catalan number
// Space Complexity: O(4^n / sqrt(n)) for storing the combinations

var generateParenthesis = function(n) {
    const dp = [];
    dp[0] = [''];

    for (let i = 1; i <= n; i++) {
        const current = [];
        for (let j = 0; j < i; j++) {
            for (let left of dp[j]) {
                for (let right of dp[i - 1 - j]) {
                    current.push("(" + left + ")" + right); // Generate combinations
                }
            }
        }
        dp[i] = current;
    }

    return dp[n];
};
```

