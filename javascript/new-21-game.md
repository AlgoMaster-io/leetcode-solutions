# [Leetcode 837: New 21 Game](https://leetcode.com/problems/new-21-game/)

## Approaches
1. [Recursive Approach without Memoization](#recursive-approach)
2. [Recursive Approach with Memoization](#recursive-memoization-approach)
3. [Dynamic Programming](#dynamic-programming-approach)

### Recursive Approach

In this approach, we explore the problem using simple recursion. The idea is to simulate the game and recursively calculate the probability of winning the game. We decide, for each current score, whether to draw an additional card or stop.

#### Intuition:
- Start with the current score `curr` being 0.
- Each step involves drawing numbers between 1 to `K`.
- For each number, calculate the probability of reaching score `curr+i` where `i` ranges from 1 to `K`.
- The process repeats until the score exceeds `N` or reaches a non-draw state.

```javascript
function new21Game(N, K, W) {
    // Helper function to use recursion to find the probability
    function helper(curr) {
        if (curr >= K) {
            // If current score is equal to or exceeds K, check win condition
            return curr <= N ? 1 : 0;
        }
        let sum = 0;
        for (let i = 1; i <= W; i++) {
            sum += helper(curr + i);
        }
        // Calculate the probability by averaging over all possible draws
        return sum / W;
    }

    // Start recursion from score 0
    return helper(0);
}

console.log(new21Game(21, 17, 10)); // Initial test call
```

**Time Complexity**: O(W^K), where W is the max draw number and K is the number of draws allowed. This is highly inefficient.

**Space Complexity**: O(K) due to the recursive call stack.


### Recursive Approach with Memoization

This approach improves upon the basic recursive approach by storing the results of subproblems to avoid redundant computations.

#### Intuition:
- Use a memoization table to store already computed probabilities for a given score.
- This reduces the overhead of recalculating probabilities for scores that have been computed previously.

```javascript
function new21Game(N, K, W) {
    const memo = Array(K + W).fill(null);
    
    function helper(curr) {
        if (curr >= K) {
            return curr <= N ? 1 : 0;
        }
        if (memo[curr] !== null) {
            return memo[curr];
        }
        
        let sum = 0;
        for (let i = 1; i <= W; i++) {
            sum += helper(curr + i);
        }
        memo[curr] = sum / W;
        return memo[curr];
    }

    return helper(0);
}

console.log(new21Game(21, 17, 10));
```

**Time Complexity**: O(K * W), where K is the maximum score before stopping and W is the max draw since each subproblem is calculated once.

**Space Complexity**: O(K + W) for the memoization table.


### Dynamic Programming Approach

This approach uses a bottom-up dynamic programming method to calculate the probabilities.

#### Intuition:
- Use an array `dp` where `dp[x]` represents the probability of having a score of `x`.
- Iterate from bottom up; if the score is less than `K`, compute probabilities using previous results.
- Use a sliding window sum to optimize calculation.

```javascript
function new21Game(N, K, W) {
    if (K === 0 || N >= K + W) return 1;
    const dp = Array(N + 1).fill(0);
    dp[0] = 1.0;
    let sumW = 1.0;
    let result = 0.0;

    for (let i = 1; i <= N; i++) {
        dp[i] = sumW / W;
        if (i < K) {
            sumW += dp[i];
        } else {
            result += dp[i];
        }
        if (i - W >= 0) {
            sumW -= dp[i - W];
        }
    }
    return result;
}

console.log(new21Game(21, 17, 10)); // Prints probability of winning
```

**Time Complexity**: O(N), where N is the maximum desired score.

**Space Complexity**: O(N) for the dynamic programming table.

By analyzing and implementing these approaches, we explore different ways to optimize the probability calculation for the New 21 Game problem. The dynamic programming approach offers a feasible and efficient solution.

