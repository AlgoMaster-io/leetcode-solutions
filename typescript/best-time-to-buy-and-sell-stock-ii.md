# [Leetcode 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Approaches:

1. [Greedy Approach](#greedy-approach)
2. [Simple Iteration and Comparison](#simple-iteration-and-comparison)

---

## Greedy Approach

### Intuition:
The problem allows for transactions (buy and sell) multiple times. A greedy approach is fitting here because we are interested in maximum gain, which can be achieved by summing up all profitable transactions. Essentially, if there's an opportunity to gain profit over consecutive increasing prices, we take it.

### Approach:
1. Traverse through the price array.
2. For each day, if the next day’s price is higher than today’s, we make a transaction (buy today, sell tomorrow) because this would net us a profit equal to the difference between these two days’ prices.
3. Accumulate all such profits.

### Code:

```typescript
function maxProfit(prices: number[]): number {
    let maxProfit = 0;
    
    for (let i = 1; i < prices.length; i++) {
        // If the current day's price is higher than the previous, take the profit
        if (prices[i] > prices[i - 1]) {
            maxProfit += prices[i] - prices[i - 1]; 
        }
    }
    
    return maxProfit;
}
```

### Complexity:
- **Time Complexity**: O(n), where n is the number of days, as we traverse the list once.
- **Space Complexity**: O(1), as no additional data structures are used.

---

## Simple Iteration and Comparison

### Intuition:
A straightforward way of looking at the problem is by considering that multiple transactions are allowed. Hence, the simplest solution is to accumulate profit every time there's an opportunity.

### Approach:
1. Initialize total profit to 0.
2. Iterate through each pair of consecutive days.
3. Compare the prices of the two days. If the later day is priced higher, sell the stock bought at the earlier day.
4. Continue this through the list and sum up total profit.

### Code:

```typescript
function maxProfit(prices: number[]): number {
    let profit = 0;
    
    for (let i = 1; i < prices.length; i++) {
        // Add profit if the price is higher today than it was yesterday
        if (prices[i] > prices[i - 1]) {
            profit += prices[i] - prices[i - 1];
        }
    }
    
    return profit;
}
```

### Complexity:
- **Time Complexity**: O(n), since the solution requires a single pass through the price array.
- **Space Complexity**: O(1), as it uses a fixed amount of extra space.

The greedy approach and this simple iteration and comparison method essentially implement the same logic but might be described or looked at slightly differently. Both provide the most efficient solution for this problem.

