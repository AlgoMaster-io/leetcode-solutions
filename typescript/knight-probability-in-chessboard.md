# [688. Knight Probability in Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard/)

## Approaches
- [Approach 1: Recursive Backtracking](#approach-1-recursive-backtracking)
- [Approach 2: Memoization](#approach-2-memoization)
- [Approach 3: Dynamic Programming (DP)](#approach-3-dynamic-programming-dp)

## Approach 1: Recursive Backtracking

### Intuition

The simplest approach is to use recursive backtracking. The idea is to recursively move the knight in all possible directions for a given number of moves, and calculate the probability recursively. This approach, while straightforward, is not efficient due to many overlapping subproblems.

### Implementation

```typescript
function knightProbability(n: number, k: number, r: number, c: number): number {
    // All possible moves for a knight
    const directions = [[2, 1], [2, -1], [-2, 1], [-2, -1], 
                        [1, 2], [1, -2], [-1, 2], [-1, -2]];
    
    // Recursive function to calculate probability
    function calculateProbability(currR: number, currC: number, movesLeft: number): number {
        // If the knight is out of bounds, probability is 0
        if (currR < 0 || currR >= n || currC < 0 || currC >= n) return 0;
        // If no more moves left, the probability is 1 (the knight is still on the board)
        if (movesLeft === 0) return 1;
        
        let probability = 0;
        // Try all possible moves
        for (const [dr, dc] of directions) {
            probability += calculateProbability(currR + dr, currC + dc, movesLeft - 1) / 8;
        }
        return probability;
    }

    return calculateProbability(r, c, k);
}
```

### Complexity

- **Time Complexity:** O(8^k), because in each step the function calls itself up to 8 times for k moves.
- **Space Complexity:** O(k) due to the recursion stack.

## Approach 2: Memoization

### Intuition

To optimize the recursive approach, we can use memoization. This involves storing the results of subproblems, meaning if a subproblem is encountered again, the stored result can be used directly without redundant calculations.

### Implementation

```typescript
function knightProbability(n: number, k: number, r: number, c: number): number {
    const directions = [[2, 1], [2, -1], [-2, 1], [-2, -1],
                        [1, 2], [1, -2], [-1, 2], [-1, -2]];
    const memo = new Map<string, number>();

    function calculateProbability(currR: number, currC: number, movesLeft: number): number {
        if (currR < 0 || currR >= n || currC < 0 || currC >= n) return 0;
        if (movesLeft === 0) return 1;
        
        const key = `${currR}-${currC}-${movesLeft}`;
        if (memo.has(key)) return memo.get(key) as number;

        let probability = 0;
        for (const [dr, dc] of directions) {
            probability += calculateProbability(currR + dr, currC + dc, movesLeft - 1) / 8;
        }
        
        memo.set(key, probability);
        return probability;
    }

    return calculateProbability(r, c, k);
}
```

### Complexity

- **Time Complexity:** O(n^2 * k), as there are at most n^2*k unique states.
- **Space Complexity:** O(n^2 * k) due to the memoization map.

## Approach 3: Dynamic Programming (DP)

### Intuition

The most efficient solution involves using Dynamic Programming (DP) to store the probabilities of the knight staying on the board after a certain number of moves in a bottom-up fashion. We use a 3D DP table where `dp[step][i][j]` represents the probability of the knight being at position `(i, j)` after `step` moves.

### Implementation

```typescript
function knightProbability(n: number, k: number, r: number, c: number): number {
    const directions = [[2, 1], [2, -1], [-2, 1], [-2, -1],
                        [1, 2], [1, -2], [-1, 2], [-1, -2]];
    
    const dp = Array.from({ length: k + 1 }, () => Array.from({ length: n }, () => Array(n).fill(0)));
    
    dp[0][r][c] = 1;
    
    for (let step = 1; step <= k; step++) {
        for (let i = 0; i < n; i++) {
            for (let j = 0; j < n; j++) {
                for (const [dr, dc] of directions) {
                    const prevR = i - dr;
                    const prevC = j - dc;
                    if (prevR >= 0 && prevR < n && prevC >= 0 && prevC < n) {
                        dp[step][i][j] += dp[step - 1][prevR][prevC] / 8;
                    }
                }
            }
        }
    }
    
    let probability = 0;
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            probability += dp[k][i][j];
        }
    }
    return probability;
}
```

### Complexity

- **Time Complexity:** O(n^2 * k), iterating over k steps, and for each step, covering the entire board.
- **Space Complexity:** O(n^2 * k) due to the 3D DP array.

By understanding each approach, you can choose the best optimization level to suit the specific needs when implementing knight movements in a chessboard scenario.

