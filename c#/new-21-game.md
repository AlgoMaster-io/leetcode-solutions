# [Leetcode 837: New 21 Game](https://leetcode.com/problems/new-21-game/)

## Approaches:
- [Approach 1: Recursive Approach with Memoization](#approach-1-recursive-approach-with-memoization)
- [Approach 2: Dynamic Programming with Sliding Window](#approach-2-dynamic-programming-with-sliding-window)

---

### Approach 1: Recursive Approach with Memoization

#### Intuition:
The problem is fundamentally a probability problem. We need to calculate the probability of getting a score less than or equal to `K` but more than or equal to `N`. A recursive approach considers each possible draw from 1 to `W`, and recursively finds the probabilities. Memoization is used to store previously calculated probabilities for specific scores to reduce redundant calculations.

#### Code:

```csharp
public class Solution {
    public double New21Game(int N, int K, int W) {
        if (K == 0 || N >= K + W - 1) return 1.0;
        
        double[] memo = new double[K + W];
        
        // Function to calculate probabilities using memoization
        double Dfs(int scores) {
            if (scores >= K) return scores <= N ? 1.0 : 0.0;
            if (memo[scores] > 0) return memo[scores];
            
            // Calculating probabilities by considering moving from scores to scores + x for 1 <= x <= W
            double probability = 0;
            for (int i = 1; i <= W; i++) {
                probability += Dfs(scores + i);
            }
            probability /= W;
            memo[scores] = probability;
            return probability;
        }
        
        return Dfs(0);
    }
}
```

#### Time and Space Complexity:
- **Time Complexity:** O(K * W). There are K states and for each state, we consider W transitions.
- **Space Complexity:** O(K + W). The memoization table stores data for `K + W` states.

---

### Approach 2: Dynamic Programming with Sliding Window

#### Intuition:
Dynamic programming can be utilized to systematically build up solutions for sub-problems (scores), utilizing solutions to smaller sub-problems. The idea is to use a sliding window to compute the probabilities more efficiently.

Instead of recomputing the subproblem solutions, we slide over the probability window which keeps track of the sum of probabilities that need consideration.

#### Code:

```csharp
public class Solution {
    public double New21Game(int N, int K, int W) {
        if (K == 0 || N >= K + W - 1) return 1.0;
        
        double[] dp = new double[N + 1];
        dp[0] = 1.0; // base case
        double sumProbabilities = 1.0; // sum of probabilities in the current window
        double result = 0.0;
        
        for (int i = 1; i <= N; i++) {
            // Calculate probability for reaching current score i
            dp[i] = sumProbabilities / W;
            
            // If the score is still less than K, contribute to future scores
            if (i < K) sumProbabilities += dp[i];
            else result += dp[i]; // If score reaches to a valid final state, add to result
            
            // Maintain the window to only include more recent W elements
            if (i >= W) sumProbabilities -= dp[i - W];
        }
        
        return result;
    }
}
```

#### Time and Space Complexity:
- **Time Complexity:** O(N). We go through scores from 0 to N.
- **Space Complexity:** O(N). We need an array to store probabilities for each score.

---
These approaches incrementally improve from a simple recursive strategy with memoization to a more efficient dynamic programming approach with sliding window optimization. This systematic breakdown also highlights the effective use of dynamic programming in probability-based problems.

