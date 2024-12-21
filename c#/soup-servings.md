### [Leetcode 808: Soup Servings](https://leetcode.com/problems/soup-servings/)

---

## Approaches

1. [Recursive Solution with Memoization](#approach-1)
2. [Dynamic Programming Solution](#approach-2)

---

## Approach 1: Recursive Solution with Memoization

### Intuition

This problem is about serving two types of soup, A and B, with four distinct ways of serving each turn. The goal is to find the probability that soup A runs out first. Given that the operations are independent and soup A and B quantities are distributed in a 25 ml serving scheme, a recursive approach can be utilized to compute these probabilities.

A brute-force approach would involve recursively computing the probability of all possible servings, but this would have a high complexity due to repetitive calculations. Therefore, memoization is crucial to store intermediate results and avoid redundant computations, thus optimizing the recursive approach.

### Steps

1. Create a recursive function with memoization to calculate the probability for given soup A and B amounts.
2. Define base cases when soup A or B is zero.
3. For each recursive call, apply all four serving actions and calculate the combined probability.
4. Use a dictionary to store previously computed results to avoid redundant computations.

### Code

```csharp
using System;
using System.Collections.Generic;

public class Solution {
    private Dictionary<(int, int), double> memo = new Dictionary<(int, int), double>();

    public double SoupServings(int N) {
        if (N > 4800) return 1.0; // The problem states that for large values of N, the probability approaches 1.
        
        return Serve(N, N);
    }

    private double Serve(int a, int b) {
        if (a <= 0 && b <= 0) return 0.5; // both run out
        if (a <= 0) return 1;             // A runs out first
        if (b <= 0) return 0;             // B runs out first

        if (memo.ContainsKey((a, b))) return memo[(a, b)];

        double probability = 0.25 * (Serve(a - 100, b) + Serve(a - 75, b - 25) + Serve(a - 50, b - 50) + Serve(a - 25, b - 75));

        memo[(a, b)] = probability;
        
        return probability;
    }
}
```

### Time Complexity

- The complexity is approximately O(n^2) due to the nature of memoized recursive function calls, where each state is evaluated once.

### Space Complexity

- O(n^2) space for the memoization storage.

---

## Approach 2: Dynamic Programming Solution

### Intuition

Convert the recursive solution into a dynamic programming solution to reduce function call overhead and achieve a more iterative understanding of the problem. Construct a DP table to calculate probabilities iteratively.

### Steps

1. Formulate a 2D array `dp` where `dp[i][j]` stores the probability of the current state when A has `i` ml and B has `j` ml.
2. Initialize base cases directly.
3. Fill the table in a bottom-up approach considering all possible serving actions.

### Code

```csharp
public class Solution {
    public double SoupServings(int N) {
        if (N > 4800) return 1.0;
        
        int size = (N + 24) / 25;
        double[,] dp = new double[size + 1, size + 1];
        
        for (int a = 0; a <= size; a++) {
            for (int b = 0; b <= size; b++) {
                if (a == 0 && b == 0) {
                    dp[a, b] = 0.5;
                } else if (a == 0) {
                    dp[a, b] = 1.0;
                } else if (b == 0) {
                    dp[a, b] = 0.0;
                } else {
                    dp[a, b] = 0.25 * (
                        GetDP(dp, a - 4, b) + 
                        GetDP(dp, a - 3, b - 1) + 
                        GetDP(dp, a - 2, b - 2) + 
                        GetDP(dp, a - 1, b - 3)
                    );
                }
            }
        }
        
        return dp[size, size];
    }

    private double GetDP(double[,] dp, int a, int b) {
        if (a < 0) a = 0;
        if (b < 0) b = 0;
        return dp[a, b];
    }
}
```

### Time Complexity

- O(n^2) since we iterate through all possible states once.

### Space Complexity

- O(n^2) for the DP table.

---

These solutions provide a detailed perspective on how to solve the problem using both recursive and iterative methodologies, optimizing through memoization and dynamic programming respectively.

