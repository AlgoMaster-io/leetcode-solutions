# [Leetcode 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approaches
1. [Recursive Approach](#recursive-approach)
2. [Recursive Approach with Memoization](#recursive-approach-with-memoization)
3. [Dynamic Programming Approach](#dynamic-programming-approach)

---

### Recursive Approach

**Intuition**:  
The problem can be approached recursively by trying to calculate the maximum profit at each decision point - whether to buy, sell, or cooldown. At each day, we can choose to buy stock, sell stock, or simply do nothing and wait for the cooldown to end. The recursion helps explore all such possibilities.

#### Code:
```python
def maxProfit(prices):
    def profit(day, can_buy):
        # Base case: no more days left
        if day >= len(prices):
            return 0
        
        if can_buy:
            # Two choices: buy the stock or cooldown
            buy = profit(day + 1, False) - prices[day]
            cooldown = profit(day + 1, True)
            return max(buy, cooldown)
        else:
            # Two choices: sell the stock or cooldown
            sell = profit(day + 2, True) + prices[day]
            cooldown = profit(day + 1, False)
            return max(sell, cooldown)
    
    # Start the recursion from day 0 with the ability to buy
    return profit(0, True)
```

#### Complexity:
- **Time Complexity**: \(O(2^n)\) where \(n\) is the number of days. This is due to the exponential number of states being considered without any optimization.
- **Space Complexity**: \(O(n)\) for the recursion stack.

---

### Recursive Approach with Memoization

**Intuition**:  
To optimize the recursive approach, we can store the results of previously solved subproblems to avoid redundant calculations. We use a memoization technique to cache the results for each state, i.e., for each day and buying/selling decision.

#### Code:
```python
def maxProfit(prices):
    memo = {}
    
    def profit(day, can_buy):
        # Base case: no more days left
        if day >= len(prices):
            return 0
        
        # Check if result is already computed
        if (day, can_buy) in memo:
            return memo[(day, can_buy)]
        
        if can_buy:
            # Two choices: buy the stock or cooldown
            buy = profit(day + 1, False) - prices[day]
            cooldown = profit(day + 1, True)
            memo[(day, can_buy)] = max(buy, cooldown)
        else:
            # Two choices: sell the stock or cooldown
            sell = profit(day + 2, True) + prices[day]
            cooldown = profit(day + 1, False)
            memo[(day, can_buy)] = max(sell, cooldown)
        
        return memo[(day, can_buy)]
    
    # Start the recursion from day 0 with the ability to buy
    return profit(0, True)
```

#### Complexity:
- **Time Complexity**: \(O(n)\) since each state (day and buy/sell) is only computed once.
- **Space Complexity**: \(O(n)\) for both memoization and recursion stack.

---

### Dynamic Programming Approach

**Intuition**:  
Using dynamic programming, we iteratively calculate the maximum profit achievable up to each day under different conditions. We use a DP table where each entry represents the maximum profit at a given day.

#### Code:
```python
def maxProfit(prices):
    if not prices:
        return 0
    
    n = len(prices)
    # dp[i][0]: maximum profit on day i with no stock in hand (could have just sold or rested)
    # dp[i][1]: maximum profit on day i with a stock in hand
    dp = [[0] * 2 for _ in range(n)]
    
    # Base cases
    dp[0][0] = 0  # We do nothing on day 0
    dp[0][1] = -prices[0]  # We buy stock on day 0
    
    for i in range(1, n):
        # Max profit not holding a stock today can't be less than not holding yesterday after a cooldown or selling today
        dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
        
        # Max profit holding a stock today can't be less than holding yesterday or buying today after cooldown
        dp[i][1] = max(dp[i-1][1], (dp[i-2][0] if i > 1 else 0) - prices[i])
    
    # The result is the max profit where on the last day we aren't holding any stock
    return dp[-1][0]
```

#### Complexity:
- **Time Complexity**: \(O(n)\) since we process each price exactly once.
- **Space Complexity**: \(O(n)\) for the DP table, can be optimized to \(O(1)\) by storing only necessary previous states.

---

Each approach provides a way to understand and solve this problem from a different perspective, demonstrating how insights in dynamic programming can help optimize recursive solutions.

