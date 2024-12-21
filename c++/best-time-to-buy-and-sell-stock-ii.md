## [Leetcode Problem 122: Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

When dealing with this problem, our goal is to decide on days we should buy and sell the stock to maximize profit. We can buy and sell multiple times but cannot hold more than one unit of stock at a time.

### Approaches

1. [Brute Force](#brute-force-approach)
2. [Greedy](#greedy-approach)

### Brute Force Approach

In this solution, we try to exhaust all possibilities of buying and selling the stock by enumerating every transaction's possible combinations. Although this gives a clear picture of the problem, it is inefficient for large input sizes.

#### Intuition

- Generate all possible transactions and calculate the profit for each sequence.
- Take the maximum profit obtained among all transaction sequences.

#### C++ Code

```cpp
#include <vector>
#include <iostream>

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        return calculate(prices, 0);
    }

private:
    int calculate(std::vector<int>& prices, int start) {
        int maxProfit = 0;
        for (int i = start; i < prices.size(); i++) {
            for (int j = i + 1; j < prices.size(); j++) {
                // If selling is profitable
                if (prices[j] > prices[i]) {
                    int profit = prices[j] - prices[i]
                        + calculate(prices, j + 1); // Recurse from the sell point
                    maxProfit = std::max(maxProfit, profit);
                }
            }
        }
        return maxProfit;
    }
};
```

#### Time and Space Complexity

- **Time Complexity:** \(O(n!)\) - This approach tries to calculate every combination of buy/sell transactions.
- **Space Complexity:** \(O(n)\) - Due to recursive stack space.

### Greedy Approach

The greedy approach optimizes the solution by making local decisions, which leads to the global optimum. The main idea is to sell immediately if selling today yields a profit.

#### Intuition

- For every price that is higher than the previous day's price, treat it as a sale, and add the profit to your running total.
- This effectively captures all the upward slopes from every local minimum to the next local maximum, enabling you to collect profit at each increase.

#### C++ Code

```cpp
#include <vector>
#include <iostream>

class Solution {
public:
    int maxProfit(std::vector<int>& prices) {
        int maxProfit = 0;
        for (int i = 1; i < prices.size(); i++) {
            // Add to profit if today's price is higher than yesterday's
            if (prices[i] > prices[i - 1]) {
                maxProfit += (prices[i] - prices[i - 1]);
            }
        }
        return maxProfit;
    }
};
```

#### Time and Space Complexity

- **Time Complexity:** \(O(n)\) - We are traveling the price list only once.
- **Space Complexity:** \(O(1)\) - No additional space used other than a few variables.

By optimizing the approach by using a greedy algorithm, we can make this problem run effectively even with the maximum constraints. This captures the essence of gaining the maximum profit without missing profitable opportunities due to dip and rise of stock prices.

