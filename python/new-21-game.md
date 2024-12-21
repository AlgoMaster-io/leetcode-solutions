# [Leetcode 837: New 21 Game](https://leetcode.com/problems/new-21-game/)

## Table of Contents
- [Approach 1: Brute Force Simulation](#approach-1-brute-force-simulation)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)

---

## Approach 1: Brute Force Simulation

### Intuition
The problem is about simulating the game and deriving the probability of reaching a score \(K\) or less. The brute force way would be to simulate all possible outcomes of the game until \(N\) times. However, this approach is impractical for large \(N\) due to exponential combinations.

### Steps
1. Simulate every possible path of game playing keeping track of points.
2. If the point value crosses \(K\), terminate that path.
3. Count all the possible paths where the game ends with a score of \(K\) or less.
4. Divide by the total number of paths to get the probability.

### Time Complexity
- **Time:** \(O(W^K)\), where we recursively calculate each combination.
- **Space:** \(O(K)\), for recursion stack.

### Code

```python
def new21Game(N: int, K: int, W: int) -> float:
    def play(points):
        if points >= K:
            return 1.0 if points <= N else 0.0
        total_probability = 0
        for draw in range(1, W + 1):
            total_probability += play(points + draw)
        return total_probability / W

    if K == 0 or N >= K + W:
        return 1.0
    
    return play(0)
```

---

## Approach 2: Dynamic Programming

### Intuition
Instead of recalculating the possibilities recursively, we can store the results of smaller subproblems using dynamic programming. This approach aims to efficiently calculate the probability the player having \(N\) or fewer points when the game stops.

### Steps
1. Create a dp array where `dp[x]` represents the probability of reaching exactly the score `x`.
2. Initialize `dp[0]` to 1 since the probability of having 0 points at the start is 100%.
3. Use the cumulative probability and sliding window technique to simplify calculations:
   - From `points` 0 to `K - 1`, calculate the cumulative sum.
   - Update `dp` using the cumulative sum divided by `W`.
   - Exit loop when beyond `K + W`.
4. Sum up probabilities from scores 0 to \(N\).

### Time Complexity
- **Time:** \(O(K + W)\), each state is calculated once through the population of the dp table.
- **Space:** \(O(K + W)\), for the dp array to store probabilities.

### Code

```python
def new21Game(N: int, K: int, W: int) -> float:
    if K == 0 or N >= K + W:
        return 1.0

    dp = [0.0] * (K + W)
    dp[0] = 1.0
    
    cumulative_sum = 0.0
    for points in range(1, K + W):
        if points <= K - 1:
            cumulative_sum += dp[points - 1]
        if points - W > 0:
            cumulative_sum -= dp[points - W - 1]
        
        dp[points] = cumulative_sum / W if points <= K else 0

    return sum(dp[K:N+1])
```

By precomputing and using dynamic programming, this solution efficiently computes the probability, even for large \(N\), \(K\), and \(W\).

