# [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Optimized One Pass Approach](#optimized-one-pass-approach)

### Brute Force Approach

The naive approach to solving this problem is to use two nested loops to try and buy stock on each day and then sell it on every day following the purchase day. This will allow us to check all possible transactions and choose the maximum profit. 

#### Intuition:

1. The outer loop will track the day we are buying the stock and the inner loop will track the day we are selling the stock.
2. Compute the profit for each pair and keep track of the maximum profit.

```javascript
var maxProfit = function(prices) {
    let maxProfit = 0;
    // Iterate over each day, considering it as the buying day
    for (let i = 0; i < prices.length; i++) {
        // Iterate over the later days, considering them as the selling day
        for (let j = i + 1; j < prices.length; j++) {
            // Calculate profit for each buy-sell pair
            let profit = prices[j] - prices[i];
            // Keep track of the maximum profit
            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }
    }
    return maxProfit;
};
```

**Time Complexity:** O(n^2) - We have to check every pair of days.

**Space Complexity:** O(1) - We only use a constant amount of space.

### Optimized One Pass Approach

This solution attempts to go through the price list just once while keeping track of the minimum buying price encountered so far and computing the profit if we were to sell the stock at the current day price. This method exploits the fact that selling must occur after buying.

#### Intuition:

1. Start by setting `minPrice` to a very large number which will be updated as we encounter lower prices.
2. Traverse through each price and at each step update the `minPrice` if the current price is lower.
3. Calculate a potential profit at each step by subtracting `minPrice` from the current price and compare it to `maxProfit`.
4. Keep track of the highest profit encountered.

```javascript
var maxProfit = function(prices) {
    let minPrice = Infinity;
    let maxProfit = 0;
    // Iterate over each day's price
    for (let i = 0; i < prices.length; i++) {
        // Update the minPrice if the current price is lower
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        }
        // Calculate potential profit from selling at current price
        else {
            let profit = prices[i] - minPrice;
            // Update maxProfit if the current profit is higher
            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }
    }
    return maxProfit;
};
```

**Time Complexity:** O(n) - We traverse the list only once.

**Space Complexity:** O(1) - No extra space is used beyond a few variables.

