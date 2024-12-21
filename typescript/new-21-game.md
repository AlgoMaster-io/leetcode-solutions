# [Leetcode 837: New 21 Game](https://leetcode.com/problems/new-21-game/)

## Approaches
- [Basic Dynamic Programming Solution](#basic-dynamic-programming-solution)
- [Optimized Dynamic Programming Using Sliding Window](#optimized-dynamic-programming-using-sliding-window)

## Basic Dynamic Programming Solution

### Intuition

To solve the New 21 Game problem, we need to find the probability that Alice's points `x` sum to a number `x <= K` before she reaches points `N`. The idea of using dynamic programming here is to build a solution incrementally. We use a DP array to track the probability of reaching every point up to `N`. The probability of reaching a point `dp[i]` is based on whether it can be achieved from previous points using a valid action (drawing a number from `1` to `W`).

### Approach

1. Initialize a DP array `dp` where `dp[i]` represents the probability of having `i` points.
2. Know that `dp[0]` is `1` because the game starts with `0` points.
3. For each possible point `i` up to `K-1`, calculate the number of ways to get there using the previous `W` points.
4. Calculate the running sum of probabilities for each point up to `K`.
5. Use this running sum to derive probabilities for higher points efficiently.
6. Finally, sum up the probabilities `dp[i]` from the points `K` to `N` to find the total probability of not exceeding `N`.

### Code

```typescript
function new21Game(N: number, K: number, W: number): number {
    if (K === 0 || N >= K + W) return 1.0;

    const dp: number[] = new Array(N + 1).fill(0);
    dp[0] = 1.0;
    
    let sumProb = 1.0;
    let result = 0.0;

    for (let i = 1; i <= N; ++i) {
        dp[i] = sumProb / W;
        
        if (i < K) {
            sumProb += dp[i];
        } else {
            result += dp[i];
        }

        if (i - W >= 0) {
            sumProb -= dp[i - W];
        }
    }

    return result;
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(N)\) because we iterate through possible scores up to N.
- **Space Complexity**: \(O(N)\) since we store probabilities for each point in the DP array.

## Optimized Dynamic Programming Using Sliding Window

### Intuition

While the basic dynamic programming approach is straightforward, it can be further optimized. Notice that the sum of probabilities for computing `dp[i]` can be maintained in a sliding window fashion. This allows us to avoid some unnecessary calculations.

### Approach

1. Use a variable `sumProb` to maintain a sliding window sum of probabilities over the last `W` elements.
2. This sliding window helps to update probabilities for each `i` efficiently within the time complexity constraint.
3. Use the same logic as before to compute the result, but with reduced computation in maintaining sumProb.

### Code

```typescript
function new21Game(N: number, K: number, W: number): number {
    if (K === 0 || N >= K + W) return 1.0;

    const dp: number[] = new Array(N + 1).fill(0.0);
    dp[0] = 1.0;
    
    let sumProb = 1.0, result = 0.0;

    for (let i = 1; i <= N; ++i) {
        dp[i] = sumProb / W;
        
        if (i < K) {
            sumProb += dp[i];
        } else {
            result += dp[i];
        }

        if (i - W >= 0) {
            sumProb -= dp[i - W];
        }
    }

    return result;
}
```

### Time and Space Complexity
- **Time Complexity**: \(O(N)\) because the problem is solved using a sliding window approach, iterating from 1 to N.
- **Space Complexity**: \(O(K)\) which is an improvement when `N` is greater than `K`, as we only need the last `K` probabilities in dp array for computation.

