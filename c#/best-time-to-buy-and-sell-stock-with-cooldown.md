The problem can be found at the following link: [Leetcode 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approaches
- [Approach 1: Recursive Solution with Memoization](#approach-1-recursive-solution-with-memoization)
- [Approach 2: Dynamic Programming (Bottom-Up)](#approach-2-dynamic-programming-bottom-up)

### Approach 1: Recursive Solution with Memoization
The problem is a variation of the "buy and sell stock" problem with an additional constraint that after selling a stock, you cannot immediately buy on the next day (cooldown of one day). This can be solved using a recursive approach with memoization to avoid recalculating the same subproblems.

**Intuition:**
- For each day, you have the option to buy, sell, or rest.
- If you buy stock, you subtract its price from your profit and move to the next day. If you sell, you add its price to your profit and move past the cooldown. If you rest, you move to the next day without changing the profit.
- Use memoization to store the results of subproblems to avoid redundant calculations.

```csharp
public class Solution {
    public int MaxProfit(int[] prices) {
        int n = prices.Length;
        int[,] memo = new int[n, 2]; // Use a 2D array where n is days and 2 states (holding or not holding)
        for(int i = 0; i < n; i++) {
            memo[i,0] = -1;
            memo[i,1] = -1;
        }
        return MaxProfitHelper(prices, 0, 0, memo);
    }

    private int MaxProfitHelper(int[] prices, int day, int holding, int[,] memo) {
        if (day >= prices.Length)
            return 0;
        
        if (memo[day, holding] != -1)
            return memo[day, holding];

        int doNothing = MaxProfitHelper(prices, day + 1, holding, memo);
        if (holding == 1) {
            // We are holding a stock, we can sell
            int sell = prices[day] + MaxProfitHelper(prices, day + 2, 0, memo);
            memo[day, holding] = Math.Max(doNothing, sell);
        } else {
            // We are not holding a stock, we can buy
            int buy = -prices[day] + MaxProfitHelper(prices, day + 1, 1, memo);
            memo[day, holding] = Math.Max(doNothing, buy);
        }
        return memo[day, holding];
    }
}
```

**Complexity Analysis:**
- Time Complexity: O(n), where n is the number of days. Each state is computed at most once due to memoization.
- Space Complexity: O(n), as we require additional space for the memoization table.

### Approach 2: Dynamic Programming (Bottom-Up)
In this approach, a bottom-up dynamic programming solution is employed to solve the problem. We maintain three states for each day: 
1. 'Sell' - Maximum profit if we sold on that day.
2. 'Buy' - Maximum profit if we bought on that day.
3. 'Cooldown' - Maximum profit if we did nothing on that day.

**Intuition:**
- 'Sell' for any day is the max of 'Buy' from the previous day plus today's price.
- 'Buy' for any day is the max of 'Cooldown' from the previous day minus today's price.
- 'Cooldown' for any day is the max of 'Sell' or 'Cooldown' from the previous day.
- 'Cooldown' handles the cooldown period explicitly, ensuring that we do not immediately buy after selling.
  
```csharp
public class Solution {
    public int MaxProfit(int[] prices) {
        if (prices.Length == 0) return 0;

        int n = prices.Length;
        int sell = 0, buy = -prices[0], cooldown = 0, preSell = 0;
        
        for (int i = 1; i < n; i++) {
            preSell = sell;
            sell = buy + prices[i];  // Sell on day i
            buy = Math.Max(buy, cooldown - prices[i]);  // Buy on day i
            cooldown = Math.Max(cooldown, preSell);  // Cooldown on day i
        }
        
        return Math.Max(sell, cooldown);  // Maximum of sell and cooldown state on last day
    }
}
```

**Complexity Analysis:**
- Time Complexity: O(n), iterating over the prices array once.
- Space Complexity: O(1), as we are only maintaining constant space for 'sell', 'buy', 'cooldown', and 'preSell'.

Each approach showcases a method of solving the problem, with Approach 2 being more optimal in terms of space usage, demonstrating efficient use of dynamic programming for such types of stock trading problems.

