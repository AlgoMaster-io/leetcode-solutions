# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)

let memo = {};

function climbStairs(n) {
    if (n <= 2) return n;
    if (memo.hasOwnProperty(n)) return memo[n];

    // Recurrence relation: f(n) = f(n-1) + f(n-2)
    const result = climbStairs(n - 1) + climbStairs(n - 2);
    memo[n] = result; // Store the result in a map to avoid recalculating
    return result;
}
```

## Approach 2: Dynamic Programming

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)

function climbStairs(n) {
    if (n <= 2) return n;

    const dp = new Array(n + 1);
    dp[1] = 1;
    dp[2] = 2;

    // Build the dp array with previous solutions
    for (let i = 3; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n]; // The answer is stored in the last element
}
```

## Approach 3: Optimized Dynamic Programming

### Solution
javascript
```javascript
// Time Complexity: O(n)
// Space Complexity: O(1)

function climbStairs(n) {
    if (n <= 2) return n;

    let first = 1;  // Represents dp[i-2]
    let second = 2; // Represents dp[i-1]
    let current = 0;

    // Use two variables to avoid full dp array and update them iteratively
    for (let i = 3; i <= n; i++) {
        current = first + second; // Current step number is sum of previous two steps
        first = second; // Move second to first
        second = current; // Update second to the current
    }

    return current; // Result is stored in current
}
```

