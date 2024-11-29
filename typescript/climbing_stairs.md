# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

## Approach 1: Recursion with Memoization

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function climbStairs(n: number): number {
    const memo: { [key: number]: number } = {};

    const helper = (n: number): number => {
        if (n <= 2) return n;
        if (memo[n] !== undefined) return memo[n];

        const result = helper(n - 1) + helper(n - 2);
        memo[n] = result; // Store the result in the map to avoid recalculating
        return result;
    };

    return helper(n);
}
```

## Approach 2: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
function climbStairs(n: number): number {
    if (n <= 2) return n;

    const dp: number[] = new Array(n + 1);
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
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(1)
function climbStairs(n: number): number {
    if (n <= 2) return n;

    let first: number = 1;  // Represents dp[i-2]
    let second: number = 2; // Represents dp[i-1]
    let current: number = 0;

    // Use two variables to avoid full dp array and update them iteratively
    for (let i = 3; i <= n; i++) {
        current = first + second; // Current step number is the sum of the previous two steps
        first = second; // Move second to first
        second = current; // Update second to the current
    }

    return current; // Result is stored in current
}
```

