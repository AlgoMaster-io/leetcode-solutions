# [Leetcode 121: Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)

## Approaches

1. [Brute Force Approach](#brute-force-approach)
2. [Optimized Single Pass Approach](#optimized-single-pass-approach)

---

### Brute Force Approach

#### Intuition:
The brute force approach involves checking the profit gained by buying the stock on each day and selling it on every day after the buy day. We compute all possible profit combinations to determine the maximum possible profit.

#### Solution:
1. Iterate over each day's price (buy day).
2. For each buy day, iterate over all following days' prices to calculate all possible sell days.
3. Compute the profit by calculating the difference between the sell and buy days.
4. Track the maximum profit encountered.

```csharp
public class Solution {
    public int MaxProfit(int[] prices) {
        int maxProfit = 0;
        // Iterate through each day as the potential "buy" day.
        for (int i = 0; i < prices.Length; i++) {
            // Try selling the stock on each subsequent day.
            for (int j = i + 1; j < prices.Length; j++) {
                // Current profit if bought on day i and sold on day j.
                int profit = prices[j] - prices[i];
                // Update the maximum profit if this one is higher.
                if (profit > maxProfit) {
                    maxProfit = profit;
                }
            }
        }
        return maxProfit;
    }
}
```

#### Time Complexity:
- O(n^2), where n is the number of days (prices array length).

#### Space Complexity:
- O(1), as no additional space is used aside from variables for tracking profit.

---

### Optimized Single Pass Approach

#### Intuition:
We can optimize by maintaining a minimum buy price up to the current day and calculate profit with the current day price as the sell day. This allows us to compute the max profit in a single pass.

#### Solution:
1. Initialize `minPrice` with a large value.
2. Traverse the prices array:
   - Update `minPrice` with the lowest price encountered so far.
   - Compute the current profit by subtracting the minPrice from the current price.
   - Track the maximum profit encountered.

```csharp
public class Solution {
    public int MaxProfit(int[] prices) {
        int maxProfit = 0;
        int minPrice = int.MaxValue;

        // Traverse through each day.
        for (int i = 0; i < prices.Length; i++) {
            // Update minPrice if the current price is lower.
            if (prices[i] < minPrice) {
                minPrice = prices[i];
            } else {
                // Calculate potential profit if selling on the current day.
                int profit = prices[i] - minPrice;
                // Update maxProfit if this potential is higher.
                if (profit > maxProfit) {
                    maxProfit = profit;
                }
            }
        }
        return maxProfit;
    }
}
```

#### Time Complexity:
- O(n), where n is the number of days (prices array length).

#### Space Complexity:
- O(1), as only a few variables are used for tracking prices and profits. 

This solution efficiently calculates the maximum profit by iteratively recording the minimum purchase price and maintaining the best profit seen so far.

