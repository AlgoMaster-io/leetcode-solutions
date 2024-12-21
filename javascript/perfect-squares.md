# [Perfect Squares - LeetCode](https://leetcode.com/problems/perfect-squares/)

## Approaches:
1. [Breadth-First Search (BFS)](#bfs-solution)
2. [Dynamic Programming](#dynamic-programming-solution)
3. [Mathematical Approach (Lagrange's Four Square Theorem)](#lagranges-theorem-solution)

### BFS Solution

The idea is to treat this problem as a graph traversal problem. We will consider each number as a node and see how to reach 0 using minimum steps by subtracting perfect squares at each step.

#### Intuition:
- At each step, subtract a perfect square from the number `n`.
- Use BFS to find the shortest path from `n` to 0, which de facto gives the minimum number of perfect squares needed.
- We start with the number itself and use all perfect squares less than or equal to it to reach 0.

```javascript
function numSquaresBFS(n) {
    if (n <= 0) return 0;
    
    const queue = [n];
    let level = 0;
    
    while (queue.length > 0) {
        level++;
        const size = queue.length;
        
        for (let i = 0; i < size; i++) {
            const current = queue.shift(); // Dequeue a number
            for (let j = 1; j * j <= current; j++) {
                let remainder = current - j * j; // Subtract perfect square
                if (remainder == 0) {
                    return level; // Found the count of perfect squares
                }
                queue.push(remainder); // Queue the remainder
            }
        }
    }
    
    return level;
}
```
- **Time Complexity**: O(n)
- **Space Complexity**: O(n)

### Dynamic Programming Solution

This approach uses dynamic programming to build up a solution using previously computed results.

#### Intuition:
- Create a dp array where `dp[i]` represents the minimum number of perfect squares needed for the sum `i`.
- Base Case: `dp[0] = 0` because 0 can be formed with 0 numbers.
- For each number `i`, compute `dp[i]` as `1 + dp[i - j^2]` for each perfect square `j^2` less than `i`.

```javascript
function numSquaresDP(n) {
    const dp = Array(n + 1).fill(Infinity);
    dp[0] = 0; // Base case
    
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j * j <= i; j++) {
            dp[i] = Math.min(dp[i], dp[i - j * j] + 1); // Use previously computed minimums
        }
    }
    
    return dp[n];
}
```
- **Time Complexity**: O(n * sqrt(n))
- **Space Complexity**: O(n)

### Lagrange's Theorem Solution

Based on Lagrange's Four Square Theorem, every natural number can be represented as the sum of four integer squares.

#### Intuition:
- Directly check if the number itself is a perfect square, if so, the answer is 1.
- Check if the number can be expressed as a sum of two perfect squares.
- Check sum of three perfect squares. Technically, if the number cannot be represented by one or two squares, it can be represented by three or four.

```javascript
function numSquaresLagrange(n) {
    if (isPerfectSquare(n)) return 1;
    
    // Check using sum of two squares
    for (let i = 1; i * i <= n; i++) {
        if (isPerfectSquare(n - i * i)) return 2;
    }
    
    // Check using three squares
    while (n % 4 == 0) n /= 4;  // Reduce n by removing factor of 4
    if (n % 8 == 7) return 4;    // Special case
    return 3;
}

function isPerfectSquare(x) {
    const s = Math.floor(Math.sqrt(x));
    return s * s === x;
}
```
- **Time Complexity**: O(sqrt(n))
- **Space Complexity**: O(1)

