# [Leetcode 123: Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

## Table of Contents
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming with 3D State](#approach-2-dynamic-programming-with-3d-state)
- [Approach 3: Optimized Dynamic Programming](#approach-3-optimized-dynamic-programming)

---

## Approach 1: Brute Force

### Intuition

The problem at hand is to determine the maximum profit we can get by making at most two transactions on given stock prices. A brute force method would involve exploring all combinations of two transactions to find the maximum possible profit. Each transaction involves buying on one day and selling on another subsequent day.

### Code

```cpp
// Brute force approach to examine all possible buy/sell days
// Time Complexity: O(n^4)
// Space Complexity: O(1)

#include <vector>
#include <algorithm>
using namespace std;

int maxProfit(vector<int>& prices) {
    int n = prices.size();
    int maxProfit = 0;

    // Explore all possible first transactions (buy/sell)
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            // Calculate profit for first transaction
            int profit1 = prices[j] - prices[i];
            // Explore all possible second transactions (buy/sell)
            for (int k = j + 1; k < n; ++k) {
                for (int l = k + 1; l < n; ++l) {
                    // Calculate profit for second transaction
                    int profit2 = prices[l] - prices[k];
                    maxProfit = max(maxProfit, profit1 + profit2);
                }
            }
        }
    }
    return maxProfit;
}
```

### Complexity Analysis

- **Time Complexity**: `O(n^4)` where `n` is the number of days. Checking every possible pair of transactions leads to this complexity.
- **Space Complexity**: `O(1)`. We only use a fixed amount of extra space irrespective of `n`.

---

## Approach 2: Dynamic Programming with 3D State

### Intuition

To improve the efficiency, we can use dynamic programming to keep track of the state over time. We define a DP table where `dp[i][j][k]` represents the maximum profit upto day `i` with `j` transactions completed and `k` states (holding or not holding a stock).

- `k = 0` means you are not holding a stock.
- `k = 1` means you are holding a stock.

### Code

```cpp
// Dynamic Programming approach using 3D State representation
// Time Complexity: O(n)
// Space Complexity: O(n)

#include <vector>
#include <algorithm>
using namespace std;

int maxProfit(vector<int>& prices) {
    if (prices.empty()) return 0;
    
    int n = prices.size();
    vector<vector<vector<int>>> dp(n, vector<vector<int>>(3, vector<int>(2, 0)));
    
    // Initialization
    dp[0][1][0] = 0;
    dp[0][1][1] = -prices[0];
    dp[0][2][0] = 0;
    dp[0][2][1] = -prices[0];
    
    for (int i = 1; i < n; ++i) {
        for (int j = 1; j <= 2; ++j) {
            // Max profit without stock on day i
            dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i]);
            // Max profit with stock on day i
            dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i]);
        }
    }
    
    return dp[n-1][2][0];
}
```

### Complexity Analysis

- **Time Complexity**: `O(n)`. We iterate through the prices in a linear fashion.
- **Space Complexity**: `O(n)`. Our DP table size is `n * 2 * 2`.

---

## Approach 3: Optimized Dynamic Programming

### Intuition

The space complexity of the previous approach can be further optimized. We observe that the DP table only requires information from the previous day. Thus, we can optimize the space used, converting it from `O(n)` to `O(1)` by using variables to track the previous state.

### Code

```cpp
// Optimized Dynamic Programming Approach with reduced space usage
// Time Complexity: O(n)
// Space Complexity: O(1)

#include <vector>
#include <algorithm>
using namespace std;

int maxProfit(vector<int>& prices) {
    int buy1 = INT_MIN, sell1 = 0;
    int buy2 = INT_MIN, sell2 = 0;

    for (const auto& price : prices) {
        buy1 = max(buy1, -price);              // Max profit after buying first stock
        sell1 = max(sell1, buy1 + price);      // Max profit after selling first stock
        buy2 = max(buy2, sell1 - price);       // Max profit after buying second stock
        sell2 = max(sell2, buy2 + price);      // Max profit after selling second stock
    }

    return sell2;
}
```

### Complexity Analysis

- **Time Complexity**: `O(n)`. We traverse the prices once.
- **Space Complexity**: `O(1)`. We use only a fixed amount of space to track the profits.

In conclusion, transitioning from a brute-force method to an optimized dynamic programming solution increases the efficiency greatly, making it feasible to handle larger inputs efficiently.

