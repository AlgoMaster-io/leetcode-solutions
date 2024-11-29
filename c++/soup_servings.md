# 808. [Soup Servings](https://leetcode.com/problems/soup-servings/)

## Approach 1: Recursive with Memoization

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
#include <unordered_map>
#include <string>

class Solution {
public:
    std::unordered_map<std::string, double> memo;

    double soupServings(int N) {
        if (N >= 5000) return 1.0; // Threshold optimization
        return serve(N, N);
    }

private:
    double serve(int a, int b) {
        if (a <= 0 && b <= 0) return 0.5; // Both soups are finished
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes

        std::string key = std::to_string(a) + "," + std::to_string(b);
        if (memo.find(key) != memo.end()) return memo[key];

        double probability = 0.25 * (
            serve(a - 100, b) +
            serve(a - 75, b - 25) +
            serve(a - 50, b - 50) +
            serve(a - 25, b - 75)
        );

        memo[key] = probability;
        return probability;
    }
};
```

## Approach 2: Iterative with Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n^2)
// Space Complexity: O(n^2)
#include <vector>

class Solution {
public:
    double soupServings(int N) {
        if (N >= 5000) return 1.0; // Threshold optimization
        N = (N + 24) / 25; // Reduce and round up

        std::vector<std::vector<double>> dp(N + 1, std::vector<double>(N + 1, 0.0));
        dp[0][0] = 0.5; // Base case: both soups are empty
        for (int i = 1; i <= N; i++) {
            dp[i][0] = 0; // Soup B empty first
            dp[0][i] = 1; // Soup A empty first
        }

        for (int i = 1; i <= N; i++) {
            for (int j = 1; j <= N; j++) {
                dp[i][j] = 0.25 * (
                    getProbability(dp, i - 4, j) +
                    getProbability(dp, i - 3, j - 1) +
                    getProbability(dp, i - 2, j - 2) +
                    getProbability(dp, i - 1, j - 3)
                );
            }
        }

        return dp[N][N];
    }

private:
    double getProbability(const std::vector<std::vector<double>>& dp, int a, int b) {
        if (a <= 0 && b <= 0) return 0.5; // Both finish at the same time
        if (a <= 0) return 1; // Only soup A finishes
        if (b <= 0) return 0; // Only soup B finishes
        return dp[a][b];
    }
};
```

