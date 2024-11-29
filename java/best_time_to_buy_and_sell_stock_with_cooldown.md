# 309. [Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approach 1: Recursive with Memoization

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        Integer[] memo = new Integer[n];
        return calculateMaxProfit(prices, 0, memo);
    }
    
    private int calculateMaxProfit(int[] prices, int currentDay, Integer[] memo) {
        if (currentDay >= prices.length) {
            return 0;
        }
        if (memo[currentDay] != null) {
            return memo[currentDay];
        }

        int maxProfit = 0;
        for (int sellDay = currentDay + 1; sellDay < prices.length; sellDay++) {
            // Calculate the profit for the current transaction
            int profit = prices[sellDay] - prices[currentDay] + calculateMaxProfit(prices, sellDay + 2, memo);
            maxProfit = Math.max(maxProfit, profit);
        }

        memo[currentDay] = Math.max(maxProfit, calculateMaxProfit(prices, currentDay + 1, memo));
        return memo[currentDay];
    }
}
```

## Approach 2: Dynamic Programming with State Variables

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(n)
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }
        int n = prices.length;

        int[] buy = new int[n];
        int[] sell = new int[n];
        int[] cooldown = new int[n];

        buy[0] = -prices[0];
        sell[0] = 0;
        cooldown[0] = 0;

        for (int i = 1; i < n; i++) {
            buy[i] = Math.max(buy[i - 1], cooldown[i - 1] - prices[i]);
            sell[i] = Math.max(sell[i - 1], buy[i - 1] + prices[i]);
            cooldown[i] = Math.max(cooldown[i - 1], sell[i - 1]);
        }

        return Math.max(sell[n - 1], cooldown[n - 1]);
    }
}
```

## Approach 3: Optimized Space Dynamic Programming

### Solution
```java
// Time Complexity: O(n)
// Space Complexity: O(1)
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }

        int buy = -prices[0];
        int sell = 0;
        int cooldown = 0;

        for (int i = 1; i < prices.length; i++) {
            int newSell = Math.max(sell, buy + prices[i]);
            int newBuy = Math.max(buy, cooldown - prices[i]);
            cooldown = Math.max(cooldown, sell);
            buy = newBuy;
            sell = newSell;
        }

        return Math.max(sell, cooldown);
    }
}
```

