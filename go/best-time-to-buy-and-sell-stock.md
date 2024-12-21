# [Leetcode 121: Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

## Approaches

- [Brute Force Approach](#brute-force-approach)
- [Optimized Linear Scan](#optimized-linear-scan)

## Brute Force Approach

### Intuition

The brute force approach considers every possible pair of days in which the stock can be bought and sold. For each possible buying day, it checks every subsequent day to calculate the profit and keeps track of the maximum profit found.

### Approach

1. Initialize `maxProfit` to 0.
2. Iterate through the array with index `i` to consider each day as a potential buying day.
3. For each `i`, iterate with index `j` over the days after `i` to consider each as a potential selling day.
4. Calculate the profit as `prices[j] - prices[i]`.
5. Update `maxProfit` if the current profit is greater.
6. Return `maxProfit`.

### Time and Space Complexity

- **Time Complexity**: O(n^2), where `n` is the number of days. This is because we are iterating over all pairs of days.
- **Space Complexity**: O(1), as the space used does not depend on the input size.

### Code

```go
func maxProfit(prices []int) int {
    maxProfit := 0
    n := len(prices)
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            profit := prices[j] - prices[i]
            if profit > maxProfit {
                maxProfit = profit
            }
        }
    }
    return maxProfit
}
```

## Optimized Linear Scan

### Intuition

Instead of considering all pairs of days, we can iterate over the prices once. During the iteration, we track the minimum price seen so far (as a potential buying price), and for each day's price, calculate the profit if sold that day. We keep track of the maximum profit found.

### Approach

1. Initialize `minPrice` to a very large number (or `prices[0]`) and `maxProfit` to 0.
2. Iterate through the price array:
   - Update `minPrice` to the smaller of the current `minPrice` or the current price `prices[i]`.
   - Calculate the current profit as `prices[i] - minPrice`.
   - Update `maxProfit` if the current profit is greater.
3. Return `maxProfit`.

### Time and Space Complexity

- **Time Complexity**: O(n), where `n` is the number of days. We pass through the list only once.
- **Space Complexity**: O(1), as we use a constant amount of additional space.

### Code

```go
func maxProfit(prices []int) int {
    minPrice := int(^uint(0) >> 1) // Set minPrice to the maximum integer value.
    maxProfit := 0

    for _, price := range prices {
        if price < minPrice {
            minPrice = price // Update minPrice if the current price is lower.
        } else if profit := price - minPrice; profit > maxProfit {
            maxProfit = profit // Update maxProfit if the current profit is greater.
        }
    }

    return maxProfit
}
```

