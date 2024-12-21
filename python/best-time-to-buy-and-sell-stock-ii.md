# [Leetcode 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## Approaches

1. [Simple One Pass](#simple-one-pass)
2. [Optimal Greedy](#optimal-greedy)

---

### 1. Simple One Pass

**Intuition:**
The basic idea here is to iterate over the prices and sum up all the profitable consecutive differences. This is because to get the maximum, we want to capture every opportunity where the price increases.

Whenever there’s an increase from one day to the next:
- We can "buy" on the previous day and "sell" on the next day.
- We accumulate the profit which is simply the difference between these two days.

**Code:**

```python
def maxProfit(prices):
    # Initialize total profit to zero
    total_profit = 0
    
    # Loop over each price (starting from the second one)
    for i in range(1, len(prices)):
        # If today's price is higher than yesterday's, we have a profit opportunity
        if prices[i] > prices[i - 1]:
            # Accumulate the profit from this opportunity
            total_profit += prices[i] - prices[i - 1]
    
    return total_profit

# Example usage:
prices = [7, 1, 5, 3, 6, 4]
print(maxProfit(prices))  # Output: 7
```

**Time Complexity:** O(n) - We are making a single pass over the prices array.

**Space Complexity:** O(1) - We use a constant amount of extra space irrespective of input size.

---

### 2. Optimal Greedy 

**Intuition:**
This method refines the Simple One Pass solution by utilizing the natural order of stock prices. Since we want to maximize the profit, we ideally want to "buy" low and "sell" high. This directly translates into summing up all the gains from every increase without actually performing real buy/sell transactions.

- Treat every price increase as a single transaction.
- For every increase from price[i] to price[i+1], add the difference to profit.

This greedy approach always takes advantage of every upward movement.

**Code:**

```python
def maxProfit(prices):
    # Initialize total profit to zero
    total_profit = 0
    
    # Loop over each price to calculate profit
    for i in range(1, len(prices)):
        # Accumulate only when there is an increase
        if prices[i] > prices[i - 1]:
            total_profit += prices[i] - prices[i - 1]
    
    return total_profit

# Example usage:
prices = [1, 2, 3, 4, 5]
print(maxProfit(prices))  # Output: 4
```

**Time Complexity:** O(n) - We make a single pass through the prices array.

**Space Complexity:** O(1) - Uses a constant amount of space.

---

Both approaches exploit the problem's flexibility to perform multiple transactions and efficiently compound the profits. The straightforward nature of the greedy solution makes it optimal for solving this problem efficiently.

