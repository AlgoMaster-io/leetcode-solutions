## [Leetcode 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

### Solutions:

1. [Recursive Approach with Memoization](#recursive-approach-with-memoization)
2. [Dynamic Programming with State Machine](#dynamic-programming-with-state-machine)

---

### Recursive Approach with Memoization

The basic idea is to use recursion to explore all combinations of buying and selling stocks, but with the added constraint of a cooldown day after selling. We'll use memoization to optimize the recursion and avoid repeated calculations.

#### Intuition:

- Consider each day, you have three options:
  1. **Buy**: Only possible if you haven't already bought a stock.
  2. **Sell**: Only possible if you've previously bought a stock.
  3. **Rest**: Skip the day. If sold the previous day, this day must be a rest day (cooldown).
- Use recursion to explore these options for each day.
- Use memoization to store results for subproblems defined by current day and holding status.

```cpp
#include <vector>
#include <cmath>
#include <algorithm>
#include <iostream>

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        std::vector<std::vector<int>> memo(prices.size(), std::vector<int>(2, -1));
        return dfs(prices, memo, 0, 0);
    }

private:
    int dfs(const std::vector<int>& prices, std::vector<std::vector<int>>& memo, int index, int holding) {
        if (index >= prices.size()) {
            return 0;
        }
        if (memo[index][holding] != -1) {
            return memo[index][holding];
        }
        
        int doNothing = dfs(prices, memo, index + 1, holding);
        if (holding) {
            int sell = prices[index] + dfs(prices, memo, index + 2, 0);  // Cooldown on selling
            memo[index][holding] = std::max(doNothing, sell);
        } else {
            int buy = -prices[index] + dfs(prices, memo, index + 1, 1);
            memo[index][holding] = std::max(doNothing, buy);
        }
        return memo[index][holding];
    }
};
```

**Time Complexity**: O(n), where n is the number of days/prices.  
**Space Complexity**: O(n), needed for the memoization array.

---

### Dynamic Programming with State Machine

This is a more optimal approach, inspired by state machines, representing each day's decision.

#### Intuition:

- **States**:
  1. **Held**: Maximum profit if we are holding a stock till this day.
  2. **Sold**: Maximum profit if we sell a stock on this day.
  3. **Rest**: Maximum profit if we didn't hold any stock this day.
- Use dynamic programming to fill these states as an array/table, based on transitions between them:
  - `held[i] = max(held[i-1], rest[i-1] - prices[i])` (Hold a stock or buy one today)
  - `sold[i] = held[i-1] + prices[i]` (Sell your stock today)
  - `rest[i] = max(rest[i-1], sold[i-1])` (Take a rest today, possibly after cooldown)

```cpp
#include <vector>
#include <algorithm>
#include <iostream>

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        if (prices.empty()) return 0;
        
        int n = prices.size();
        std::vector<int> held(n, 0), sold(n, 0), rest(n, 0);
        
        held[0] = -prices[0];
        sold[0] = 0;
        rest[0] = 0;
        
        for (int i = 1; i < n; ++i) {
            held[i] = std::max(held[i - 1], rest[i - 1] - prices[i]);
            sold[i] = held[i - 1] + prices[i];
            rest[i] = std::max(rest[i - 1], sold[i - 1]);
        }
        
        return std::max(sold[n - 1], rest[n - 1]);
    }
};
```

**Time Complexity**: O(n), due to single pass through the prices vector.  
**Space Complexity**: O(n), for storing states of each day. However, it can be optimized to O(1) by using only variables for current states since each state only relies on the previous day.

Use these insights to choose the best approach based on constraints and performance goals. The DP approach is generally preferred due to its clear structure and optimal performance.

