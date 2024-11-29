# 808. [Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approach 1: Recursive with Memoization

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
using System.Collections.Generic;

public class Solution {
    private Dictionary<string, double> memo = new Dictionary<string, double>();

    public double SoupServings(int N) {
        if (N >= 5000) return 1.0; // Threshold optimization
        return Serve(N, N);
    }

    private double Serve(int a, int b) {
        if (a <= 0 && b <= 0) return 0.5; // Both soups are finished
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes
        
        string key = a + "," + b;
        if (memo.ContainsKey(key)) return memo[key];

        double probability = 0.25 * (
            Serve(a - 100, b) +
            Serve(a - 75, b - 25) +
            Serve(a - 50, b - 50) +
            Serve(a - 25, b - 75)
        );

        memo[key] = probability;
        return probability;
    }
}
```

## Approach 2: Iterative with Dynamic Programming

### Solution
csharp
```csharp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
public class Solution {
    public double SoupServings(int N) {
        if (N >= 5000) return 1.0; // Threshold optimization
        N = (N + 24) / 25; // Reduce and round up

        double[,] dp = new double[N + 1, N + 1];
        dp[0, 0] = 0.5; // Base case: both soups are empty
        for (int i = 1; i <= N; i++) {
            dp[i, 0] = 0; // Soup B empty first
            dp[0, i] = 1; // Soup A empty first
        }

        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                dp[i, j] = 0.25 * (
                    GetProbability(dp, i - 4, j) +
                    GetProbability(dp, i - 3, j - 1) +
                    GetProbability(dp, i - 2, j - 2) +
                    GetProbability(dp, i - 1, j - 3)
                );
            }
        }

        return dp[N, N];
    }

    private double GetProbability(double[,] dp, int a, int b) {
        if (a <= 0 && b <= 0) return 0.5; // Both finish at the same time
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes
        return dp[a, b];
    }
}
```

