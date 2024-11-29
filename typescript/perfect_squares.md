# 279. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approach 1: Dynamic Programming

### Solution
typescript
```typescript
// Time Complexity: O(n * sqrt(n))
// Space Complexity: O(n)
class Solution {
    numSquares(n: number): number {
        const dp = new Array(n + 1).fill(0);
        dp[0] = 0; // Zero can be represented by zero squares
        
        // Start filling dp array
        for (let i = 1; i <= n; i++) {
            let min = Number.MAX_SAFE_INTEGER;
            for (let j = 1; j * j <= i; j++) {
                min = Math.min(min, dp[i - j * j] + 1);
            }
            dp[i] = min;
        }
        
        return dp[n]; // Result is stored in dp[n]
    }
}
```

## Approach 2: Breadth-First Search (BFS)

### Solution
typescript
```typescript
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
    numSquares(n: number): number {
        const queue: number[] = [];
        const visited = new Array(n + 1).fill(0);
        
        queue.push(0);
        visited[0] = 1;
        
        let level = 0;
        while (queue.length > 0) {
            const size = queue.length;
            level++;
            for (let i = 0; i < size; i++) {
                const sum = queue.shift()!;
                for (let j = 1; sum + j * j <= n; j++) {
                    const next = sum + j * j;
                    if (next === n) {
                        return level;
                    }
                    if (visited[next] === 0) {
                        queue.push(next);
                        visited[next] = 1;
                    }
                }
            }
        }
        
        return level;
    }
}
```

## Approach 3: Legendre's Three-Square Theorem

### Solution
typescript
```typescript
// Time Complexity: O(sqrt(n))
// Space Complexity: O(1)
class Solution {
    numSquares(n: number): number {
        // Check if n is a perfect square
        if (this.isPerfectSquare(n)) return 1;
        
        // Check the result is 4 using Legendre's Theorem
        while (n % 4 === 0) {
            n /= 4;
        }
        if (n % 8 === 7) return 4;
        
        // Check if it can be decomposed into sum of two squares
        for (let i = 1; i * i <= n; i++) {
            if (this.isPerfectSquare(n - i * i)) return 2;
        }
        
        return 3;
    }
    
    private isPerfectSquare(n: number): boolean {
        const s = Math.floor(Math.sqrt(n));
        return s * s === n;
    }
}
```

