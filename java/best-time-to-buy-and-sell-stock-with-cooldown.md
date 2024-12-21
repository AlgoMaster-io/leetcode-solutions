[Leetcode Problem - Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

## Approaches:
1. [Recursive Brute Force](#recursive-brute-force)
2. [Memoization](#memoization)
3. [Dynamic Programming](#dynamic-programming)
4. [Space Optimized Dynamic Programming](#space-optimized-dynamic-programming)

### Recursive Brute Force

Intuition:
- The idea here is to simulate each possible action (buy, sell, cooldown) at each day and keep track of the maximum profit.
- For each day, we can either:
  1. Not make any transaction (cooldown).
  2. Buy a stock (if currently not holding one).
  3. Sell a stock (only if currently holding a stock).
- We solve this using recursion by exploring each decision for each day and computing the maximum profit.

```java
public class Solution {
    public int maxProfit(int[] prices) {
        return maxProfitRecursive(prices, 0, false);
    }
    
    private int maxProfitRecursive(int[] prices, int index, boolean holding) {
        // Base case - no more days to trade
        if (index >= prices.length) return 0;
        
        // Cooldown (do nothing)
        int cooldown = maxProfitRecursive(prices, index + 1, holding);
        
        if (holding) {
            // If holding, we can sell
            int sell = prices[index] + maxProfitRecursive(prices, index + 2, false);
            return Math.max(cooldown, sell);
        } else {
            // If not holding, we can buy
            int buy = -prices[index] + maxProfitRecursive(prices, index + 1, true);
            return Math.max(cooldown, buy);
        }
    }
}
```

- **Time Complexity**: O(2^n), where n is the number of days. We branch into two recursive calls for each day.
- **Space Complexity**: O(n), the stack space used for recursion.

---

### Memoization

Intuition:
- The recursive solution computes the same subproblems multiple times, which increases time complexity.
- We can store the results of subproblems in a 2D array to avoid recalculating them, turning our approach into a dynamic programming solution.

```java
public class Solution {
    public int maxProfit(int[] prices) {
        int[][] memo = new int[prices.length][2];
        for (int i = 0; i < memo.length; i++) {
            Arrays.fill(memo[i], -1);
        }
        return maxProfitMemo(prices, 0, 0, memo);
    }

    private int maxProfitMemo(int[] prices, int index, int holding, int[][] memo) {
        if (index >= prices.length) return 0;
        if (memo[index][holding] != -1) return memo[index][holding];
        
        int cooldown = maxProfitMemo(prices, index + 1, holding, memo);

        if (holding == 1) {
            int sell = prices[index] + maxProfitMemo(prices, index + 2, 0, memo);
            memo[index][holding] = Math.max(cooldown, sell);
        } else {
            int buy = -prices[index] + maxProfitMemo(prices, index + 1, 1, memo);
            memo[index][holding] = Math.max(cooldown, buy);
        }
        
        return memo[index][holding];
    }
}
```

- **Time Complexity**: O(n), because each subproblem is solved at most once.
- **Space Complexity**: O(n), for the memo array.

---

### Dynamic Programming

Intuition:
- Instead of recursion, we use dynamic programming arrays to keep track of the profits.
- We define two arrays: `sell[i]` which is the maximum profit we can have up to day `i` (inclusive) and must sell on `i`, and `buy[i]` for transactions where max profit up to `i` and must buy on `i`.

```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length <= 1) return 0;
        
        int n = prices.length;
        int[] sell = new int[n];
        int[] buy = new int[n];
        
        // Initial state
        buy[0] = -prices[0]; // Buy on the first day
        sell[0] = 0;         // Can't sell on the first day
        
        for (int i = 1; i < n; i++) {
            sell[i] = Math.max(sell[i - 1], buy[i - 1] + prices[i]);
            buy[i] = Math.max(buy[i - 1], (i >= 2 ? sell[i - 2] - prices[i] : -prices[i]));
        }
        
        return sell[n - 1];
    }
}
```

- **Time Complexity**: O(n), because we iterate over the prices once.
- **Space Complexity**: O(n), due to the `buy` and `sell` arrays.

---

### Space Optimized Dynamic Programming

Intuition:
- We can notice that to compute `buy[i]` and `sell[i]`, we only need the values for `i-1` and `i-2`.
- Therefore, instead of maintaining arrays for `buy` and `sell`, we maintain only variables for the last two and current states.

```java
public class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) return 0;

        int n = prices.length;
        int sellPrev = 0, sellPrevPrev = 0;
        int buyPrev = -prices[0];
        
        for (int i = 1; i < n; i++) {
            int sellCurr = Math.max(sellPrev, buyPrev + prices[i]);
            int buyCurr = Math.max(buyPrev, sellPrevPrev - prices[i]);

            // Update previous states
            sellPrevPrev = sellPrev;
            sellPrev = sellCurr;
            buyPrev = buyCurr;
        }
        
        return sellPrev;
    }
}
```

- **Time Complexity**: O(n), since we iterate through the prices once.
- **Space Complexity**: O(1), as we are using a fixed number of variables instead of arrays.

These approaches provide a thorough understanding of trading under cooldown constraints and range from basic recursive solutions to more optimal space-efficient dynamic programming solutions.

