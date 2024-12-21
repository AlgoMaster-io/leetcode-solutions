# [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Approaches
1. [Greedy Approach](#greedy-approach)
2. [Dynamic Programming Approach - State Machine](#dynamic-programming-approach)

---

## Greedy Approach

### Intuition:
The key idea here is to maximize the profit by capturing all upward price movements in the stock price. Instead of thinking about the best time to buy and sell, we can simply focus on buying the stock when we detect an upward trend (when today's price is lower than tomorrow's price).

### Solution:
When the prices go upwards, we can consider it as a potential transaction. Therefore, we accumulate profit any time there is a price increase from one day to the next by adding the difference to the total profit.

```javascript
function maxProfit(prices) {
    let maxProfit = 0;
    for (let i = 1; i < prices.length; i++) {
        // If today's price is lower than tomorrow's, buy today and sell tomorrow
        if (prices[i] > prices[i - 1]) {
            maxProfit += prices[i] - prices[i - 1];
        }
    }
    return maxProfit;
}
```

### Time Complexity: 
- **O(n)**, where n is the number of days, since we make a single pass through all the prices.
  
### Space Complexity:
- **O(1)**, as we are using a constant amount of extra space.

---

## Dynamic Programming Approach - State Machine

### Intuition:
This approach takes inspiration from the state machine concept where we think in terms of states - holding a stock or not. We define a dynamic programming array to keep track of the maximum profit on each day given the two states - having a stock or not having a stock.

### Solution:

Let's define:
- `dp[i][0]`: Maximum profit on day `i` when not holding a stock.
- `dp[i][1]`: Maximum profit on day `i` when holding a stock.

The recursive relations:
- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])`
    - We do nothing or sell the stock if we hold it.
- `dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])`
    - We do nothing or buy the stock if we don't hold it.

```javascript
function maxProfit(prices) {
    let n = prices.length;
    if (n <= 1) return 0;

    // Initialize on first day
    let dp0 = 0; // max profit if we don't have stock
    let dp1 = -prices[0]; // max profit if we have stock
    
    for (let i = 1; i < n; i++) {
        // Transition to the next day
        let newDp0 = Math.max(dp0, dp1 + prices[i]);
        let newDp1 = Math.max(dp1, dp0 - prices[i]);
        dp0 = newDp0;
        dp1 = newDp1;
    }
    
    // End with no stock in hand
    return dp0;
}
```

### Time Complexity:
- **O(n)**, as we go through the price days once.
  
### Space Complexity:
- **O(1)**, since we are optimizing the DP solution to use constant space.

---

These approaches illustrate how understanding the nature of price movements or modeling problems as state-transition systems can significantly impact problem-solving in algorithmic interviews.

