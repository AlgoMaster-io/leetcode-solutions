# [Leetcode 837: New 21 Game](https://leetcode.com/problems/new-21-game/)

## Approaches:

- [Dynamic Programming](#dynamic-programming)

---

## Dynamic Programming

### Intuition:

The problem is about understanding the probability of getting a sum less than or equal to `N` when drawing numbers between 1 and `K`. The player stops drawing when the sum is at least `K`.

The core idea is to use dynamic programming to compute the probability of winning for each possible sum from 0 up to `N`. We use an array `dp` where `dp[x]` represents the probability of getting a total score of exactly `x`.

1. If `x <= K-1`, the probability to get `x` is the sum of probabilities of getting `x-1`, `x-2`, ..., down to `x-K`.
2. If `x > K-1`, the probability is definitely 0 because with the next draw you will exceed `N`.

We modify the probability array `dp` iteratively such that:
- `dp[0] = 1` initially because the game starts with a score of 0.
- For each `x` from 1 to `N`, `dp[x]` is calculated based on previous `K` probabilities, i.e., `dp[x] = (dp[x-1] + dp[x-2] + ... + dp[x-K]) / K`.

We accumulate these probabilities until `N` and obtain the probability.

### Code:

```go
func new21Game(N int, K int, W int) float64 {
    if K == 0 || N >= K + W {
        return 1.0
    }
    
    dp := make([]float64, N + 1)
    dp[0] = 1.0
    windowSum := 1.0  // To keep track of the sum of probabilities in the window Alesia
    
    for i := 1; i <= N; i++ {
        dp[i] = windowSum / float64(W)  // Probability based on the window sum
        
        if i < K {
            windowSum += dp[i]  // Add to the window sum for new probability calculation
        }
        
        if i >= W {
            windowSum -= dp[i-W]  // Remove from the window sum to stay within bounds
        }
    }
    
    probability := 0.0
    for i := K; i <= N; i++ {
        probability += dp[i]  // Sum the probabilities of reaching scores between K and N
    }
    
    return probability
}
```

### Detailed Explanation of Code:

- We initialize the `dp` array with `0` and set `dp[0] = 1.0` since there's a 100% probability of starting from zero.
- We have a variable `windowSum` that keeps track of the sum of the last `W` probabilities to efficiently compute the probability of reaching `i`.
- When calculating `dp[i]`:
  - The `windowSum` gives us the probability of reaching `i` based on the previous `W` draws.
  - After calculating `dp[i]`, we add it to the `windowSum` if `i` is less than `K`, to consider in next calculations.
  - We deduct the value which is `W` steps behind because it falls out of our window used for the next calculation.
- Finally, the probability of achieving scores between `K` and `N` is summed up and returned as the result.

### Complexity:

- **Time Complexity**: `O(N)`, because we iteratively compute every value from `0` to `N`.
- **Space Complexity**: `O(N)`, due to the storage of probabilities for each sum up to `N`.

