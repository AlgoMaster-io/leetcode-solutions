[Leetcode 123: Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

### Approaches:
- [Approach 1: Brute Force](#approach-1-brute-force)
- [Approach 2: Dynamic Programming](#approach-2-dynamic-programming)
- [Approach 3: Optimized Dynamic Programming](#approach-3-optimized-dynamic-programming)

---

### Approach 1: Brute Force
This naive solution checks all possible pairs of buy/sell days for up to two transactions and calculates their profit. It's computationally expensive.

#### Intuition:
1. Use nested loops to check every buy and sell opportunity.
2. For each pair, calculate the profit.
3. For the second transaction, start from after the first sell day.

#### Steps:
- Loop through each day `i` where you might buy for the first transaction.
- Loop through each day `j` after `i` where you might sell for the first transaction.
- Loop through each day `k` after `j` for the second buy day.
- Loop through each `m` after `k` for the second sell day.
- Calculate and maintain the maximum profit.

```python
def maxProfit(prices):
    n = len(prices)
    max_profit = 0
    
    # Try all possible pairs for the first transaction
    for i in range(n-1):
        for j in range(i+1, n):
            profit1 = prices[j] - prices[i]
            # Try all possible pairs for the second transaction
            for k in range(j+1, n-1):
                for m in range(k+1, n):
                    profit2 = prices[m] - prices[k]
                    # Calculate total profit for both transactions
                    total_profit = profit1 + profit2
                    max_profit = max(max_profit, total_profit)
    
    return max_profit
```

- **Time Complexity:** O(n^4)
- **Space Complexity:** O(1)

---

### Approach 2: Dynamic Programming
This method uses dynamic programming to keep track of maximum profits with a single or two transactions.

#### Intuition:
1. Use dynamic programming to store intermediate results.
2. Maintain a backward pass to capture maximum prices after a given day for the optimal second transaction.

#### Steps:
- Use arrays to store maximum profit up to day `i` with `first_buy` and `second_buy`.
- For each day, update:
  - The smallest price before day `i` and compute maximum profit for the first transaction.
  - The maximum profit if a second transaction starts after `i`.

```python
def maxProfit(prices):
    if not prices:
        return 0
    
    n = len(prices)
    left_profits = [0] * n
    right_profits = [0] * (n + 1)
    
    # Forward pass, calculate max profit from 0 to i
    min_price = prices[0]
    for i in range(1, n):
        min_price = min(min_price, prices[i])
        left_profits[i] = max(left_profits[i-1], prices[i] - min_price)
        
    # Backward pass, calculate max profit from i to end
    max_price = prices[-1]
    for i in range(n - 2, -1, -1):
        max_price = max(max_price, prices[i])
        right_profits[i] = max(right_profits[i + 1], max_price - prices[i])
        
    # Combine profits
    max_profit = 0
    for i in range(n):
        max_profit = max(max_profit, left_profits[i] + right_profits[i + 1])
        
    return max_profit
```

- **Time Complexity:** O(n)
- **Space Complexity:** O(n)

---

### Approach 3: Optimized Dynamic Programming
This method further optimizes the space by using variables instead of arrays to keep track of profits.

#### Intuition:
1. Optimize space by storing only essential state values.
2. Keep track of four key variables during a single pass.

#### Steps:
- Initialize variables:
  - `first_buy`, `first_sell` for the first transaction profit.
  - `second_buy`, `second_sell` for the second transaction profit.
- Traverse prices:
  - Update these variables to reflect profits after each buy or sell operation.

```python
def maxProfit(prices):
    if not prices:
        return 0
    
    # Variables to store the maximum profit up to the current day
    first_buy = second_buy = float('-inf')
    first_sell = second_sell = 0
    
    for price in prices:
        # Buying at the lowest price
        first_buy = max(first_buy, -price)
        # Selling for the first time
        first_sell = max(first_sell, first_buy + price)
        # Buying for the second transaction
        second_buy = max(second_buy, first_sell - price)
        # Selling for the second time
        second_sell = max(second_sell, second_buy + price)
        
    return second_sell
```

- **Time Complexity:** O(n)
- **Space Complexity:** O(1)

Each step optimizing from approach 1 to 3 results in vastly improved performance, especially moving from a time complexity of O(n^4) in naive brute force to linear time O(n) with optimized dynamic programming, making it suitable for larger datasets.

