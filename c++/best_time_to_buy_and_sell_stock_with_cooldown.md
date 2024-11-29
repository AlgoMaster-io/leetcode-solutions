# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<int> memo(n, -1);
        return calculateMaxProfit(prices, 0, memo);
    }

private:
    int calculateMaxProfit(vector<int>& prices, int currentDay, vector<int>& memo) {
        if (currentDay >= prices.size()) {
            return 0;
        }
        if (memo[currentDay] != -1) {
            return memo[currentDay];
        }

        int maxProfit = 0;
        for (int sellDay = currentDay + 1; sellDay < prices.size(); ++sellDay) {
            // Calculate the profit for the current transaction
            int profit = prices[sellDay] - prices[currentDay] + calculateMaxProfit(prices, sellDay + 2, memo);
            maxProfit = max(maxProfit, profit);
        }

        memo[currentDay] = max(maxProfit, calculateMaxProfit(prices, currentDay + 1, memo));
        return memo[currentDay];
    }
};
```

## Approach 2: Dynamic Programming with State Variables

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(n)
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) {
            return 0;
        }
        int n = prices.size();

        vector<int> buy(n, 0);
        vector<int> sell(n, 0);
        vector<int> cooldown(n, 0);

        buy[0] = -prices[0];
        sell[0] = 0;
        cooldown[0] = 0;

        for (int i = 1; i < n; ++i) {
            buy[i] = max(buy[i - 1], cooldown[i - 1] - prices[i]);
            sell[i] = max(sell[i - 1], buy[i - 1] + prices[i]);
            cooldown[i] = max(cooldown[i - 1], sell[i - 1]);
        }

        return max(sell[n - 1], cooldown[n - 1]);
    }
};
```

## Approach 3: Optimized Space Dynamic Programming

### Solution
```cpp
// Time Complexity: O(n)
// Space Complexity: O(1)
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) {
            return 0;
        }

        int buy = -prices[0];
        int sell = 0;
        int cooldown = 0;

        for (int i = 1; i < prices.size(); ++i) {
            int newSell = max(sell, buy + prices[i]);
            int newBuy = max(buy, cooldown - prices[i]);
            cooldown = max(cooldown, sell);
            buy = newBuy;
            sell = newSell;
        }

        return max(sell, cooldown);
    }
};
```

