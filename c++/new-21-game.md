# [Leetcode 837: New 21 Game](https://leetcode.com/problems/new-21-game/)

## Approaches
1. [Brute Force Simulation](#brute-force-simulation)
2. [Dynamic Programming with Memoization](#dynamic-programming-with-memoization)
3. [Optimal Dynamic Programming with Sliding Window](#optimal-dynamic-programming-with-sliding-window)

---

### Brute Force Simulation

The brute force simulation simply tries to simulate each round of the game until reaching or exceeding the score. Although this method can conceptually solve the problem, it is computationally infeasible for large `N` and `K` due to exponential time complexity.

#### Intuition:
Simulate all possible outcomes by taking each decision, i.e., adding a number between `1` and `W` to the score until I win or lose. Sum up the probabilities of winning.

#### Code:
```cpp
// This is not optimal and is only for understanding.
double new21GameBF(int N, int K, int W) {
    if (K == 0 || N >= K + W - 1) return 1.0;
    return helper(0, N, K, W) / pow(W, K);
}

double helper(int currentPoints, int N, int K, int W) {
    if (currentPoints >= K) return currentPoints <= N;
    
    double probabilitySum = 0.0;
    for (int i = 1; i <= W; ++i) {
        probabilitySum += helper(currentPoints + i, N, K, W);
    }
    return probabilitySum;
}
```
#### Time Complexity: 
- O(W^K)
#### Space Complexity: 
- O(K), due to the recursion call stack.

### Dynamic Programming with Memoization

Instead of recalculating the probability for each score repeatedly in the recursion, we can use memoization to store results of subproblems.

#### Intuition:
Use a DP table where `dp[x]` represents the probability of reaching score `x`. Iteratively calculate the probability from `K` to `N`.

#### Code:
```cpp
double new21Game(int N, int K, int W) {
    if (K == 0 || N >= K + W - 1) return 1.0;

    // dp[i] is the probability of getting a score of i
    std::vector<double> dp(K + W, 0);
    double sum = 0.0;

    for (int i = K; i <= N; ++i) {
        dp[i] = 1.0; // Any score >= K and <= N is a winning state
    }

    for (int i = K - 1; i >= 0; --i) {
        // The probability of reaching score i
        dp[i] = sum / W;
        sum = sum + dp[i] - dp[i + W];
    }
    return dp[0];
}
```
#### Time Complexity: 
- O(N + W)
#### Space Complexity: 
- O(K + W)

### Optimal Dynamic Programming with Sliding Window

Using a sliding window to optimize space complexity by calculating the sum in a linear pass without the need to store each result.

#### Intuition:
Reduce the space complexity by maintaining only a sliding window sum which moves as the scores change.

#### Code:
```cpp
double new21Game(int N, int K, int W) {
    if (K == 0 || N >= K + W - 1) return 1.0;

    std::vector<double> dp(N + 1, 0.0);
    dp[0] = 1.0;
    double windowSum = 1.0;
    double result = 0.0;

    for (int i = 1; i <= N; ++i) {
        dp[i] = windowSum / W;
        if (i < K) {
            windowSum += dp[i];
        } else {
            result += dp[i];
        }

        if (i - W >= 0) {
            windowSum -= dp[i - W];
        }
    }

    return result;
}
```
#### Time Complexity: 
- O(N)
#### Space Complexity: 
- O(N)

The optimal solution leverages both a sliding window and dynamic programming to efficiently store and calculate probabilities, ultimately solving the problem in linear time and space.

