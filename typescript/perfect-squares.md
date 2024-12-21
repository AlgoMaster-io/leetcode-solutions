# [Leetcode 279: Perfect Squares](https://leetcode.com/problems/perfect-squares/)

## Approaches:
- [Approach 1: Dynamic Programming](#approach-1-dynamic-programming)
- [Approach 2: Breadth-First Search (BFS)](#approach-2-breadth-first-search-bfs)
- [Approach 3: Mathematical Approach - Lagrange's Four Square Theorem](#approach-3-mathematical-approach-lagranges-four-square-theorem)

---

## Approach 1: Dynamic Programming

### Intuition:
The idea here is to use dynamic programming to break down the problem. We define a DP array `dp` where `dp[i]` represents the minimum number of perfect squares that sum up to `i`. We initialize the array with a maximum number (e.g., `Infinity`) because we are searching for the minimum value. The value `dp[0]` is `0` because zero requires no perfect squares to sum to zero.

### Steps:
1. Create an array `dp` of size `n+1` initialized with `Infinity`, except `dp[0] = 0`.
2. Iterate over each number `i` from `1` to `n`.
3. For each `i`, check every perfect square number `j*j` less than or equal to `i`.
4. Update `dp[i]` as the minimum value between the current `dp[i]` and `dp[i - j * j] + 1` (we use one more square `j*j`).
5. Return `dp[n]` as the answer.

### Code:
```typescript
function numSquares(n: number): number {
    // Create a dp array with size n + 1 and initialize it with Infinity
    const dp: number[] = new Array(n + 1).fill(Infinity);
    // Zero is made out of zero squares
    dp[0] = 0;

    // Iterate through numbers from 1 to n to fill the dp array
    for (let i = 1; i <= n; i++) {
        // For each i, check every perfect square number j*j
        for (let j = 1; j * j <= i; j++) {
            // Update dp[i] with the minimal value we can achieve
            dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
        }
    }
    // Return the minimum number of perfect squares that sum to n
    return dp[n];
}
```

### Complexity Analysis:
- **Time Complexity:** \(O(n \times \sqrt{n})\) - The outer loop runs `n` times and the inner loop runs up to \(\sqrt{n}\) times.
- **Space Complexity:** \(O(n)\) - The space required for the dp array.

---

## Approach 2: Breadth-First Search (BFS)

### Intuition:
BFS is useful for finding the shortest path in an unweighted graph. In this problem, each sum `i` can be thought of as a node, and edges connect to sums formed by adding a perfect square `j*j`. Thus, BFS can help find the shortest path to form `n` using the fewest perfect square numbers, similar to finding the smallest number of steps in a path.

### Steps:
1. Use a queue to process the sums, starting with `0`.
2. Start a level-wise BFS, using perfect squares as steps to reach `n`.
3. Each time you reach the number `n`, return the depth of BFS, which represents the minimal number of perfect squares needed.

### Code:
```typescript
function numSquaresBFS(n: number): number {
    const perfectSquares: number[] = [];
    const queue: number[] = [0];
    const visited: Set<number> = new Set<number>();
    
    // Collect all perfect squares less than n
    for (let i = 1; i * i <= n; i++) {
        perfectSquares.push(i * i);
    }

    let level = 0;
    
    // BFS
    while (queue.length) {
        level++;
        const size = queue.length;

        // Process each level
        for (let i = 0; i < size; i++) {
            const current = queue.shift() as number;

            // Try all perfect squares
            for (const square of perfectSquares) {
                const sum = current + square;
                
                if (sum === n) return level; // Found a solution
                
                if (sum > n) break; // Impossible to form further valid sum
                
                if (!visited.has(sum)) {
                    visited.add(sum);
                    queue.push(sum);
                }
            }
        }
    }
    return level;
}
```

### Complexity Analysis:
- **Time Complexity:** \(O(n \times \sqrt{n})\) - Each node number can have up to \(\sqrt{n}\) edges.
- **Space Complexity:** \(O(n)\) - For the queue and visited set.

---

## Approach 3: Mathematical Approach - Lagrange's Four Square Theorem

### Intuition:
Lagrange's Four Square Theorem states that every natural number can be represented as the sum of four integer squares. This means that the answer for any `n` is at most 4. This approach leverages this theorem for an additional fast computation or as a theoretical check after BFS or DP to improve performance in real scenarios.

### Steps:
1. Check if the number is itself a perfect square, then return 1.
2. Reduce the number by factor 4 (Legendre's transformation) if possible.
3. Test if it results as two squares in the form of `a^2 + b^2`, return 2.
4. Check if it can be expressed as the sum of three squares, return 3.
5. Otherwise, return 4 as per Lagrange's theorem.

### Code:
```typescript
function isPerfectSquare(x: number): boolean {
    const s = Math.sqrt(x);
    return s === Math.floor(s);
}

function numSquaresMath(n: number): number {
    // Apply Legendre's transformation
    while (n % 4 === 0) {
        n /= 4;
    }
    if (n % 8 === 7) {
        return 4;
    }

    if (isPerfectSquare(n)) {
        return 1;
    }

    for (let i = 1; i * i <= n; i++) {
        if (isPerfectSquare(n - i * i)) {
            return 2;
        }
    }
    
    return 3;
}
```

### Complexity Analysis:
- **Time Complexity:** \(O(\sqrt{n})\) - checking a few conditions and iterations.
- **Space Complexity:** \(O(1)\) - Using a fixed amount of additional space.

