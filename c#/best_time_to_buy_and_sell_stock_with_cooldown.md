# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;

public class Solution {
    public int MaxProfit(int[] prices) {
        int n = prices.Length;
        int?[] memo = new int?[n];
        return CalculateMaxProfit(prices, 0, memo);
    }
    
    private int CalculateMaxProfit(int[] prices, int currentDay, int?[] memo) {
        if (currentDay >= prices.Length) {
            return 0;
        }
        if (memo[currentDay] != null) {
            return memo[currentDay].Value;
        }

        int maxProfit = 0;
        for (int sellDay = currentDay + 1; sellDay < prices.Length; sellDay++) {
            // Calculate the profit for the current transaction
            int profit = prices[sellDay] - prices[currentDay] + CalculateMaxProfit(prices, sellDay + 2, memo);
            maxProfit = Math.Max(maxProfit, profit);
        }

        memo[currentDay] = Math.Max(maxProfit, CalculateMaxProfit(prices, currentDay + 1, memo));
        return memo[currentDay].Value;
    }
}

## Approach 2: Dynamic Programming with State Variables

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(n)
using System;

public class Solution {
    public int MaxProfit(int[] prices) {
        if (prices.Length == 0) {
            return 0;
        }
        int n = prices.Length;

        int[] buy = new int[n];
        int[] sell = new int[n];
        int[] cooldown = new int[n];

        buy[0] = -prices[0];
        sell[0] = 0;
        cooldown[0] = 0;

        for (int i = 1; i < n; i++) {
            buy[i] = Math.Max(buy[i - 1], cooldown[i - 1] - prices[i]);
            sell[i] = Math.Max(sell[i - 1], buy[i - 1] + prices[i]);
            cooldown[i] = Math.Max(cooldown[i - 1], sell[i - 1]);
        }

        return Math.Max(sell[n - 1], cooldown[n - 1]);
    }
}

## Approach 3: Optimized Space Dynamic Programming

### Solution
c#
// Time Complexity: O(n)
// Space Complexity: O(1)
using System;

public class Solution {
    public int MaxProfit(int[] prices) {
        if (prices.Length == 0) {
            return 0;
        }

        int buy = -prices[0];
        int sell = 0;
        int cooldown = 0;

        for (int i = 1; i < prices.Length; i++) {
            int newSell = Math.Max(sell, buy + prices[i]);
            int newBuy = Math.Max(buy, cooldown - prices[i]);
            cooldown = Math.Max(cooldown, sell);
            buy = newBuy;
            sell = newSell;
        }

        return Math.Max(sell, cooldown);
    }
}

