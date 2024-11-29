# 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approach 1: Dynamic Programming

### Solution
```javascript
// Time Complexity: O(n * sqrt(n))
// Space Complexity: O(n)
var numSquares = function(n) {
    const dp = new Array(n + 1).fill(Infinity);
    dp[0] = 0; // Zero can be represented by zero squares

    // Start filling dp array
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j * j <= i; j++) {
            dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
        }
    }

    return dp[n]; // Result is stored in dp[n]
};
```

## Approach 2: Breadth-First Search (BFS)

### Solution
```javascript
// Time Complexity: O(n)
// Space Complexity: O(n)
var numSquares = function(n) {
    const queue = [0];
    const visited = new Array(n + 1).fill(false);
    visited[0] = true;
    
    let level = 0;
    while (queue.length > 0) {
        const size = queue.length;
        level++;
        for (let i = 0; i < size; i++) {
            const sum = queue.shift();
            for (let j = 1; sum + j * j <= n; j++) {
                const next = sum + j * j;
                if (next == n) {
                    return level;
                }
                if (!visited[next]) {
                    queue.push(next);
                    visited[next] = true;
                }
            }
        }
    }
    
    return level;
};
```

## Approach 3: Legendre's Three-Square Theorem

### Solution
```javascript
// Time Complexity: O(sqrt(n))
// Space Complexity: O(1)
var numSquares = function(n) {
    // Check if n is a perfect square
    if (isPerfectSquare(n)) return 1;
    
    // Check the result is 4 using Legendre's Theorem
    while (n % 4 === 0) {
        n /= 4;
    }
    if (n % 8 === 7) return 4;
    
    // Check if it can be decomposed into sum of two squares
    for (let i = 1; i * i <= n; i++) {
        if (isPerfectSquare(n - i * i)) return 2;
    }
    
    return 3;
};

function isPerfectSquare(n) {
    const s = Math.sqrt(n) | 0;
    return s * s === n;
}
```

