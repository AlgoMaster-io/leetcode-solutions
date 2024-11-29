# 123. [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Approach 1: Dynamic Programming with State Variables

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int hold1 = INT_MIN, hold2 = INT_MIN;
        int release1 = 0, release2 = 0;

        for (int price : prices) {
            // First transaction
            release1 = max(release1, hold1 + price);  // Sell stock held by hold1
            hold1 = max(hold1, -price);               // Buy stock

            // Second transaction
            release2 = max(release2, hold2 + price);  // Sell stock held by hold2
            hold2 = max(hold2, release1 - price);     // Buy stock after first sell
        }

        return release2; // Maximum profit from two transactions
    }
};
```

## Approach 2: Dynamic Programming with 2D DP Array

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty() || prices.size() <= 1) return 0;

        int k = 2; // Maximum number of transactions
        int n = prices.size();
        // dp[i][j] = maximum profit using at most i transactions by time j
        vector<vector<int>> dp(k + 1, vector<int>(n, 0));

        for (int i = 1; i <= k; i++) {
            int maxDiff = -prices[0];
            for (int j = 1; j < n; j++) {
                // Previous maximum profit obtained with `i` transactions or profit after selling stock by day `j`
                dp[i][j] = max(dp[i][j - 1], prices[j] + maxDiff);
                // Max profit after buying stock on day `j`
                maxDiff = max(maxDiff, dp[i - 1][j] - prices[j]);
            }
        }
        return dp[k][n - 1]; // Maximum profit with at most `k` transactions at the last day
    }
};
```

## Approach 3: Forward and Backward Pass

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n <= 1) return 0;

        vector<int> leftProfits(n, 0);
        vector<int> rightProfits(n, 0);

        int minPrice = prices[0];
        for (int i = 1; i < n; i++) {
            minPrice = min(minPrice, prices[i]);
            leftProfits[i] = max(leftProfits[i - 1], prices[i] - minPrice);
        }

        int maxPrice = prices[n - 1];
        for (int i = n - 2; i >= 0; i--) {
            maxPrice = max(maxPrice, prices[i]);
            rightProfits[i] = max(rightProfits[i + 1], maxPrice - prices[i]);
        }

        int maxProfit = 0;
        for (int i = 0; i < n; i++) {
            maxProfit = max(maxProfit, leftProfits[i] + rightProfits[i]);
        }

        return maxProfit;
    }
};
```

