[Leetcode 121: Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Approaches:
1. [Brute Force](#brute-force)
2. [Single Pass with Min Price Tracker](#single-pass-with-min-price-tracker)

---

### 1. Brute Force

#### Intuition:
The brute force solution involves checking every possible pair of days (buy day and sell day) to calculate the profit, and keeping track of the maximum profit encountered. This approach examines each combination of days to determine the best buy and sell day by maximizing the profit.

#### Code:
```typescript
function maxProfit(prices: number[]): number {
    let maxProfit = 0;
    
    // Iterate over each day as a potential buy day
    for (let i = 0; i < prices.length; i++) {
        // Iterate over each day after the buy day as a potential sell day
        for (let j = i + 1; j < prices.length; j++) {
            // Calculate the profit if bought on day 'i' and sold on day 'j'
            const profit = prices[j] - prices[i];
            // Update maxProfit if we found a better profit
            if (profit > maxProfit) {
                maxProfit = profit;
            }
        }
    }
    
    return maxProfit;
}
```

#### Time Complexity:
- **O(n^2):** We are using two nested loops, leading to a quadratic time complexity.

#### Space Complexity:
- **O(1):** We only use a constant amount of extra space.

---

### 2. Single Pass with Min Price Tracker

#### Intuition:
We can optimize the solution by scanning through the list of prices once while keeping track of the minimum price observed so far, updating the maximum profit accordingly. This method removes the need for nested loops while achieving the same result.

#### Code:
```typescript
function maxProfit(prices: number[]): number {
    if (prices.length === 0) return 0;
    
    let minPrice = Infinity; // Initialize the minimum price to a very large number
    let maxProfit = 0;
    
    // Traverse through each price once
    for (let i = 0; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            // Update the minPrice if current price is lower than the previously recorded minPrice
            minPrice = prices[i];
        } else if (prices[i] - minPrice > maxProfit) {
            // Calculate profit if sold on day 'i' and update maxProfit if it exceeds the previous maxProfit
            maxProfit = prices[i] - minPrice;
        }
    }
    
    return maxProfit;
}
```

#### Time Complexity:
- **O(n):** We go through the array only once, thus ensuring linear time complexity.

#### Space Complexity:
- **O(1):** We use a constant amount of additional space.

Both approaches solve the problem, but the single-pass solution is more efficient and elegant, making it preferable for larger input sizes.

