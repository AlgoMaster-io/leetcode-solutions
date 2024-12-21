# [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

## Table of Contents
1. [Brute Force Approach](#brute-force-approach)
2. [One Pass Approach](#one-pass-approach)

---

### Brute Force Approach

#### Intuition
The naive solution to this problem is to evaluate all possible transactions (for each day, evaluate the profit for every following day) and track the maximum profit. However, this method is computationally expensive as it requires checking all pairs of days.

#### Code

```python
def maxProfit(prices):
    max_profit = 0
    n = len(prices)
    
    # Check every pair (i, j) where i < j
    for i in range(n):
        for j in range(i + 1, n):
            # Calculate profit of selling on day j, buying on day i
            profit = prices[j] - prices[i]
            
            # Update maximum profit if this transaction is better
            if profit > max_profit:
                max_profit = profit
                
    return max_profit
```

#### Time Complexity
- **O(n^2)** where n is the length of the `prices` array, due to the nested loops.

#### Space Complexity
- **O(1)** as we only use a fixed amount of extra space for variables such as `max_profit`.

---

### One Pass Approach

#### Intuition
We can maintain a running track of the minimum price so far and compute the potential profit for each day by subtracting the current price from this minimum price. This allows us to determine the maximum profit in a single pass through the array.

#### Code

```python
def maxProfit(prices):
    max_profit = 0
    min_price = float('inf')
    
    # Traverse through the list once
    for price in prices:
        # Update the minimum price encountered so far
        if price < min_price:
            min_price = price
            
        # Calculate current potential profit
        # by selling at the current price
        profit = price - min_price
        
        # Update the maximum profit so far if needed
        if profit > max_profit:
            max_profit = profit
            
    return max_profit
```

#### Time Complexity
- **O(n)** where n is the length of the `prices` array, as we only traverse the list once.

#### Space Complexity
- **O(1)** since we only use a constant amount of extra space.

