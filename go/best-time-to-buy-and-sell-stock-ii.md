# [Leetcode 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Solutions

- [Approach 1: Peak Valley Approach](#approach-1-peak-valley-approach)
- [Approach 2: Simple One Pass](#approach-2-simple-one-pass)

### Approach 1: Peak Valley Approach

**Intuition:**

The problem is about maximizing the profit of stock trading where you can buy and sell stocks multiple times. To solve it, we can utilize the peak-valley approach, which involves buying at a local minimum (valley) and selling at a local maximum (peak).

The core idea is to traverse the price array and look for profits by identifying valleys (local minima) and peaks (local maxima).

1. Traverse the price array:
   - Whenever a valley is found (prices[i] < prices[i-1]), consider it as a buying opportunity.
   - Continue to find a peak (prices[i] > prices[i+1]), which can be the selling point.
   - Add the profit from this transaction (peak price - valley price) to the total profit. 

**Solution Code:**

```go
func maxProfit(prices []int) int {
    // Total profit accumulator
    profit := 0
    i := 0

    // Traverse through all prices
    for i < len(prices) - 1 {
        // Find the next valley (local minimum)
        for i < len(prices) - 1 && prices[i] >= prices[i+1] {
            i++
        }
        valley := prices[i]

        // Find the next peak (local maximum)
        for i < len(prices) - 1 && prices[i] <= prices[i+1] {
            i++
        }
        peak := prices[i]

        // Add the profit from this transaction
        profit += peak - valley
    }

    return profit
}
```

**Time Complexity:** O(n), where n is the number of days in the `prices` array. We make a single pass through the list.

**Space Complexity:** O(1), as we are using a constant amount of extra space.

### Approach 2: Simple One Pass

**Intuition:**

Another intuitive solution to this problem involves taking profit whenever there is an increase in the price from one day to the next. This is a simplified greedy approach where you continuously accumulate profits from consecutive day-to-day increases.

1. Traverse each day and if the price on the next day is greater than the price of the current day, simulate a trading action, i.e., buy on the current day and sell on the next day.
2. Accumulate this profit since whenever prices[i] < prices[i+1], it means there's a profit opportunity.

**Solution Code:**

```go
func maxProfit(prices []int) int {
    // Total profit accumulator
    profit := 0

    // Traverse through price list
    for i := 1; i < len(prices); i++ {
        // If current day's price is higher than previous day's
        if prices[i] > prices[i-1] {
            // Collect the profit by aggregating
            profit += prices[i] - prices[i-1]
        }
    }

    return profit
}
```

**Time Complexity:** O(n), where n is the number of days in the `prices` array. We precisely iterate once through the list.

**Space Complexity:** O(1), as no extra space is required beyond a fixed number of variables.

Both approaches are optimal in this context but provide a different view of tackling the problem based on trading strategies. The main difference is the level of abstraction each one represents.

